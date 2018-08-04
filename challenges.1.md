# Single Container Loop Challenge
## Why
Containers can make certain aspects of a developer's or admin's life very easy by hiding complexity and by providing reliability.
In this chapter you will get a basic experience in working with containers. For this chapter we concentrate on single container applications running locally first and in Azure Container Instances in the second step.

## Here's what you'll learn: ##
- Container basics
    - Get a feeling for work with containers and understand their purpose
    - Understand what a Dockerfile is
    - How to create a container image
    - How to run a container image locally
    - Get a sense for container networking and ports
    - How to create new versions of images
    - Learn about tagging
    - How to use VSTS automation to set up an automated workflow
- Deployment
    - How to provide a container image in a registry 
    - How to set up a container registry
    - How to run a container in the cloud


## 1. Containerize your app 
- Get the code of the hello world application, i.e. git clone this repository locally and navigate to the folder `phoenix\apps\aci-helloworld`.
- Create a container image locally (you need docker running on your machine). Don't forget the trailing "." in the following line!
    ```
    docker build -t helloworld .
    ```
- Check if the image has been built.
    ```
    docker images
    ```
- Run the image in a container locally on your machine. Remember to open up the correct port in your command (-p).
    ```
    docker run -d -p 8080:80 helloworld
    ```
- Open the browser and navigate to the application you just started with your browser (http://localhost:8080). If you're running on a Linux VM in Azure, just run this command to avoid working with a graphical browser:
    ```
    wget http://localhost:8080
    ```
    Then check the content with:
    ```
    cat index.html
    ```
- Check the running processes
```
docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
bc4b6b155c2c        helloworld          "/bin/sh -c 'node /u…"   12 seconds ago      Up 9 seconds        0.0.0.0:8080->8080/tcp   peaceful_mccarthy
```
- Kill the process to clean up
```
docker kill bc4b6b155c2c
```


## 2. Automate your build 
> Need help? Check hints [here :blue_book:](hints/TeamServicesContainerBuild.md)!
- Import the sample code from to your VSTS Team Project. You can do this via UI. 
- Use VSTS to create a build definition which triggers on code changes. The build definition should 
    - create a new container image     
    - use the build number as tag to identify your image. The buildId can be found in variable *$(Build.BuildId)*  (The screenshots may show Buildnumber - make sure to use the BuildId)
    - push the new image to your private Azure Container Registry (if you don't have an ACR, create one first)

## 3. Release to ACI manually
> Need help? Check hints [here :blue_book:](hints/ManualReleaseToACI.md)!
- Run your newly created image in Azure Container Instances to see if everything works. You can start it manually in the portal or via command line.


## 4. Relase to ACI via VSTS
> Need help? Check hints [here :blue_book:](hints/TeamServicesToACI.md)!
- Use VSTS to create a release definition which is triggered by your build definition. This release definition should
    - deploy the latest image created by your build definition to ACI. Use the Azure CLI 
    task.
- Now you have a full end to end flow for single container applications.


