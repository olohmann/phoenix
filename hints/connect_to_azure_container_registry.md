# Connect an Azure Container Registry to AKS

Details see here: [Grant AKS access to ACR](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-auth-aks#grant-aks-access-to-acr)

# Connect an Arbitrary Kubernetes Cluster to Azure Container Registry or from another Subscription

!!! This is only required if you are not using an azure container registry that is part of the same subscription
!!! Prefered approach is to grant the Kubernetes service principal Reader role permissions in your azure container registry via Access Control (IAM

```sh
kubectl create secret docker-registry kuberegistry --docker-server 'myveryownregistry-on.azurecr.io' --docker-username 'username' --docker-password 'password' --docker-email 'example@example.com'
```

or

```sh
kubectl create secret docker-registry kuberegistry --docker-server $REGISTRY_URL --docker-username $REGISTRY_NAME --docker-password $REGISTRY_PASSWORD --docker-email 'example@example.com'
```

Hint: In case of problems pulling your images, try creating the secret without '' around the values.