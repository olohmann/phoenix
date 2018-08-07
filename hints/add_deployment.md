# Create a Deplyoment

## Write the Deployment Config File

1. Create a file `frontend-deployment.yml`.
2. Start with the basic template found in the Kubernetes Documentation: <https://kubernetes.io/docs/concepts/workloads/controllers/deployment/>
3. You should now build a combination of the simple pod definition and the deployment metadata.

The full `frontend-deployment.yml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: calcfrontend-deployment
  labels:
    app: calcfrontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: calcfrontend
  template:
    metadata:
      labels:
        app: calcfrontend
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
            - name: "INSTRUMENTATIONKEY"
              valueFrom:
                secretKeyRef:
                  name: appinsightsecret
                  key: appinsightskey
            - name: "PORT"
              value: "80"
```

## Apply the Deployment

1. First make sure you have no frontend pods running anymore. Use `kubectl get pods` and `kubectl delete pods/foo`.
2. Apply the Deployment `kubectl apply -f ./frontend-deployment.yml`.

## Repeat for Backend

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: calcbackend-deployment
  labels:
    app: calcbackend
spec:
  replicas: 1
  selector:
    matchLabels:
      app: calcbackend
  template:
    metadata:
      labels:
        app: calcbackend
    spec:
      containers:
        - name: calcbackend
          image: labuser01.azurecr.io/js-calc-backend:latest
          ports:
            - containerPort: 80
              name: http
              protocol: TCP
          env:
            - name: "INSTRUMENTATIONKEY"
              valueFrom:
                secretKeyRef:
                  name: appinsightsecret
                  key: appinsightskey
            - name: "PORT"
              value: "80"
```

## Explore the Results

1. Use `kubectl get pods` - you should see replicas of the frontend service.
2. Kill one pod via `kubectl delete pods/[POD-ID]`.
3. What do you observe?
