# Kubernetes yaml file deployments

## Yaml file definition

https://kubernetes.io/docs/user-guide/walkthrough/

The simplest pod definition describes the deployment of a single container. For example, an nginx web server pod might be defined as such:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.7.9
    ports:
    - containerPort: 80
```

## liveness probes and variables

https://kubernetes-v1-4.github.io/docs/user-guide/liveness/

Ensure that health checks are performed against your instance

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  #namespace: default
spec:
  containers:
  - name: nginx
    image: nginx:1.7.9
    ports:
    - containerPort: 80
    env:       
      - name: "SOMEVARIABLE"
        value: "somevalue"
    livenessProbe:
      httpGet:
        path: /
        port: 80
      initialDelaySeconds: 15
      timeoutSeconds: 1
```

## Referencing images from your own registry

To reference an image from your own registry you need to reference a credential for the cluster to login. Check the hint about secrets: [here :blue_book:](connect_to_azure_container_registry.md)

```yaml
apiVersion: "v1"
kind: Pod
metadata:
  name: somePodName
  labels:
    name: someLabelName
spec:
  containers:
    - name: blue
      image: someRegistryOnAzure.azurecr.io/someImage:latest
      ports:
        - containerPort: 80
          name: http
          protocol: TCP 
  imagePullSecrets:
    - name: nameOfYourSecret
```
