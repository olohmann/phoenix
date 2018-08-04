Team Services Configuration to build & push a docker image to your Azure Container Registry

1. [Here](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-portal) is how you create an Azure Container Registry in the Azure portal if you don't have one yet.


2. Import the sample code from to your VSTS Team Project. For example:

```sh
cp -r aci-helloworld ~/labuser01/
cd ~/labuser01/
git add --all
git commit -m "Added aci-helloworld sample code"
git push
```

3. Create a new build definition you can do this in the Build & Release tab. Choose the submenu "Builds". Create an empty build definition. Choose "Hosted Linux Preview" as build agent queue.

![](images/newbuilddefinition.png)
![](images/newbuilddefinition2.png)
![](images/newbuilddefinition3.png)
![](images/newbuilddefinition4.png)


5. Add the following tasks for your build phase (if you don't find them you can search or install from the marketplace)
**Hint:** Use $(Build.DefinitionName):$(Build.BuildId) to name your image automatically in a format that will allow you find it later. 
**Hint:** Make sure you reference your Dockerfile correctly

    - "Docker" task to create a container image
    ![](images/vstshelloworldbuildimage.png)
    - "Docker" task to push the image
    ![](images/vstshelloworldpushimage.png)
    - Enable the CI trigger
    ![](images/vsts_enable_ci.png)

6. Later you will need an additional step to move a "*.yaml" file to an artifacts folder. Use the "Publish Build Artifacts" Task. This will look like shown below.
**Hint:** Make sure you reference your yaml file from the correct location! 
![](images/vstsdropyaml.jpg)
