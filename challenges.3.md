# Kubernetes Multicontainer Challenge

In this chapter you will create a multi-container appliation in Kubernetes. This is more close to real-life but makes administration a little more challenging. In reality you might want to be able to specify that mutliple containers are able to talk to each other in a defined way. You might want to make make sure certain parts of your application run in multiple instances to cover load. You might want to be able to monitor performance of your application. You might want to make sure that your system is self-healing so that faulty components are replaced automatically. For updates you might want to make sure to have zero downtime of your application. We are going to configure all of this in this section.

## Here's what you'll learn

- How to write Yaml files to specify a desired state of a Kubernetes object
- How to use the Azure Portal to view Application performance
- How to store secrets in Kubernetes
- How to configure your Kubernetes instance to ensure a certain number of pods is always running
- How to define rolling upgrades to avoid downtime during an application update
- How to put all what you've learned into an end to end DevOps pipeline via VSTS

## Kubernetes multi container app deployment

> Need help? Check hints [here :blue_book:](hints/k8sMulti.md)!
- Get the sample code for a multi container application. See apps/js-frontend-calc, apps/js-backend-calc.
- Build the container images for frontend and backend services locally.
- Push the images to your Azure Container Registry (ACR).
- If not done yet, make sure that your AKS cluster can talk to the Azure Container Regristry (see [hint](hints/connect_to_azure_container_registry.md)).
- Apply the container images in your Kubernetes cluster as pods and wrap them in services. Create according *.yml/*.yaml files:
  - backend-pod.yml
  - backend-svc.yml
  - frontend-pod.yml
  - frontend-svc.yml
- Configure your application to be accessible from the internet and call the page. Use the calculation.

## Integrate with Application Insights

> Need help? Check hints [here :blue_book:](hints/applicationinsights.md)!
In this chapter you will create an application insights resource for monitoring your application performance and health status.

- Create application insights in azure
- Configure your apps to inject the application insights key via environment variables
- Use a kubernetes secret to ensure consistency
- Set up an availability test for your endpoint
- Observe and query your application performance during deployments and rolling upgrades

## Simple Pod vs. Deployments

Let's see what happens if one of your pods fails.

- Delete the frontend pod using the commandline and call the website again.
- You'll recognize that it will no longer work.

Let's configure it for self-healing / rolling upgrade.

> Need help? Check hints [here :blue_book:](hints/add_deployment.md)!
- Create a new yaml file **frontend-deployment.yml** and configure it to take care of replication of your application frontend pods. Set the number of replicas to 3. You can find a deployment sample in the [K8s Docs](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/).
- This new file will basically a mix of the new deployment resource type and the pod definition file.
- It will take care of starting new instances whenever one of your pods fails. Try to kill the application again by deleting frontend pods and see if your website stays responsive.
- Give it a try and kill some pods. They will be recreated.
- Check the number of backend pods. K8s will take care to keep the number of available pods as specified.

Repeat the same steps for the backend deployment.

## Fully automated VSTS YAML deployment

In this chapter you will leverage your VSTS pipeline to trigger a deployment to your K8s cluster. Your application will have no downtime during a rolling upgrade.

> Need help? Check hints [here :blue_book:](hints/TeamServicesToK8s.md)!

### Create one Converged YAML

Copy deplyoment and service definitions into a single YAML file (for front- and backend) and separte the definitions by `----` in tha YAML.

Now apply the deployment once manually.

### Build the Pipeline in VSTS

TBD

## Bonus Challenge - Technology Shootout

Let's say a co-worker of you recommends writing the backend app with in "Go" for performance reasons. How could you try the Go-Backend and run it without downtime? Where could you find performance data? 
Implement the solution and upgrade your application to the Go-backend without downtime. (The Go backend app can be found in /apps/go-calc-backend.)

- Build the Go backend image 
- Publish the image in your registry
- Modify your backend-service Yaml to target the new image
- Deploy
- Check monitoring data for performance impact
- Use helm charts to deploy continous via vsts hints [here :blue_book:](hints/TeamServicesHelmK8s.md)!
