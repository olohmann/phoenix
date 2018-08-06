# Kubernetes Multicontainer

## Import the Source Code

Copy the sources from the apps folder to your work directory (e.g. mapped git repo)

```sh
cp -r ~/phoenix/apps/js-calc-frontend ~/labuser01/
cp -r ~/phoenix/apps/js-calc-backend ~/labuser01/
```

## Build the Container Images Local

First, you must authorize your docker build host against the Azure Container Registry in order to push the built images.

```sh
az acr login --name labuser01
```

Now build and push the images:

```sh
cd ~/labuser01/js-calc-frontend
docker build --tag labuser01.azurecr.io/js-calc-frontend .
docker push labuser01.azurecr.io/js-calc-frontend

cd ~/labuser01/js-calc-backend
docker build --tag labuser01.azurecr.io/js-calc-backend .
docker push labuser01.azurecr.io/js-calc-backend
```

## Deploy to Kubernetes

### Calc Backend

#### backend-pod.yml

```yaml
apiVersion: "v1"
kind: Pod
metadata:
  name: calcbackend
  labels:
    name: calcbackend
    app: calculator
spec:
  containers:
    - name: calcbackend
      image: labuser01.azurecr.io/js-calc-backend:latest
      ports:
        - containerPort: 80
          name: http
          protocol: TCP
```

#### backend-svc.yml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: calcbackendsvc
  labels:
    name: calcbackendsvc
    app: calculator  
spec:
  ports:
  - port: 80
    name: calcbackend
    targetPort: 80
  selector:
    name: calcbackend
    app: calculator
```

### Calc Frontend

#### frontend-pod.yml

```yaml
apiVersion: "v1"
kind: Pod
metadata:
  name: calcfrontend
  labels:
    name: calcfrontend
    app: calculator
spec:
  containers:
    - name: calcfrontend
      image: labuser01.azurecr.io/js-calc-frontend:latest
      ports:
        - containerPort: 80
          name: http
          protocol: TCP
      env:
        - name: "ENDPOINT"
          value: "calcbackendsvc"
```

#### frontend-svc.yml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: calcfrontendsvc
  labels:
    name: calcfrontendsvc
    app: calculator
spec:
  selector:
    name: calcfrontend
  ports:
   - port: 80
     name: http
     targetPort: 80
     protocol: TCP
```

## Expose Public endpoint via LoadBalancer

Redeploy `frontend-svc.yaml` with the updated `type: LoadBalancer` line:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: calcfrontendsvc
  labels:
    name: calcfrontendsvc
    app: calculator
spec:
  selector:
    name: calcfrontend
  type: LoadBalancer
  ports:
   - port: 80
     name: http
     targetPort: 80
     protocol: TCP
```