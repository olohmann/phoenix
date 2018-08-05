# Deploying Secrets
https://kubernetes.io/docs/concepts/configuration/secret/

Simple way to deploy secrets via command line

```sh
kubectl create secret generic mySecretName --from-literal=username=someRandomSecretValue
```

OR do it via yaml files  - here secrets must be base64 encoded.

```sh
echo -n "someRandomSecretValue" | base64
```

To create an application insights secret required for the calculator enter the following with the correct key

```sh
kubectl create secret generic appinsightsecret --from-literal=appinsightskey=12345678
```

Define secret in yaml file

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mySecretName
type: Opaque
data:
  username: c29tZVJhbmRvbVNlY3JldFZhbHVl
```

Deploy secret to cluster

```sh
kubectl create -f secrets.yml
```

# Referencing a secret from an environment variable

Assuming you have deployed your secret to your cluster you can now reference your secret during deployments and set the value of a secret to an environment variable like this:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-env-pod
spec:
  containers:
  - name: mycontainer
    image: redis
    env:
      - name: SECRET_USERNAME
        valueFrom:
          secretKeyRef:
            name: mySecretName
            key: mySecretLookUpKey
```
