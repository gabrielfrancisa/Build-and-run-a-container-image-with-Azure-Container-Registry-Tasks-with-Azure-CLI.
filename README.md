# Build-and-run-a-container-image-with-Azure-Container-Registry-Tasks-with-Azure-CLI.
This lab, i build a container image from application code and push it to Azure Container Registry using Azure CLI. I learn how to prepare my app for containerization, create an ACR instance, and store my container image in Azure.



Tasks performed in this project:
1. Create an Azure Container Registry resource
2. Build and push an image from a Dockerfile
3. Verify the results
4. Run the image in the Azure Container Registry


>>> Create an Azure Container Registry resource
In your browser, navigate to the Azure portal https://portal.azure.com, and sign in with your Azure credentials if prompted.

Use the [>_] button to the right of the search bar at the top of the page to create a new Cloud Shell in the Azure portal, selecting a Bash environment. The cloud shell provides a command-line interface in a pane at the bottom of the Azure portal. If you are prompted to select a storage account to persist your files, select No storage account required, your subscription, and then select Apply.

Note: If you have previously created a cloud shell that uses a PowerShell environment, switch it to Bash.

Create a resource group for the resources needed for this exercise. Replace myResourceGroup with a name you want to use for the resource group. You can replace eastus with a region near you if needed. If you already have a resource group you want to use, proceed to the next step.

In this project, our Resource Group has already been created (myResourceGroup), and you may safely skip this step.

#BASH 1.
>>> az group create --location eastus --name myResourceGroup

Run the following command to create a basic container registry. 
The registry name must be unique within Azure and contain 5-50 alphanumeric characters. Replace myResourceGroup with the name you used earlier, and myContainerRegistry with a unique value.

#BASH 2.

>>> az acr create --resource-group myResourceGroup \
    >> --name mycontainerregistry54825403 --sku Basic

Note: The command creates a Basic registry, a cost-optimised option for developers learning about Azure Container Registry.


#Build and push an image from a Dockerfile
Next, you build and push an image based on a Dockerfile.

#BASH 3.
>>> echo FROM mcr.microsoft.com/hello-world > Dockerfile

Run the following az acr build command, which builds the image and, after the image is successfully built, pushes it to your registry. Replace myContainerRegistry with the name you created earlier.

#BASH 4
>>> az acr build --image sample/hello-world:v1  \
    >> --registry mycontainerregistry54825403 \
    >> --file Dockerfile .


```
possible output
    registry: myContainerRegistry.azurecr.io
    repository: sample/hello-world
    tag: v1
    digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a
  runtime-dependency:
    registry: mcr.microsoft.com
    repository: hello-world
    tag: latest
    digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a
  git: {}


Run ID: cf1 was successful after 11s
Verify the results
Run the following command to list the repositories in your registry. Replace myContainerRegistry with the name you created earlier.
```

#BASH 5.
>>> az acr repository list --name mycontainerregistry54825403 --output table

```Output:
----------------
sample/hello-world
Run the following command to list the tags on the sample/hello-world repository. Replace myContainerRegistry with the name you used earlier.
```

#BASH 6
>>> az acr repository show-tags --name mycontainerregistry54825403 \
    >>> --repository sample/hello-world --output table

```Output:
--------
v1
Run the image in the ACR
Run the sample/hello-world:v1 container image from your container registry with the az acr run command. The following example uses $Registry to specify the registry where you run the command. Replace myContainerRegistry with the name you used earlier.
```

#BASH 7.
>>> az acr run --registry mycontainerregistry54825403 \
    >>>--cmd '$Registry/sample/hello-world:v1' /dev/null

The cmd parameter in this example runs the container in its default configuration, but cmd supports other docker run parameters or even other docker commands.

```output is shortened:

Packing source code into tar to upload...
Uploading archived source code from '/tmp/run_archive_ebf74da7fcb04683867b129e2ccad5e1.tar.gz'...
Sending context (1.855 KiB) to registry: mycontainerre...
Queued a run with ID: cab
Waiting for an agent...
YEAR/03/19 19:01:53 Using acb_vol_60e9a538-b466-475f-9565-80c5b93eaa15 as the home volume
..../03/19 19:01:53 Creating Docker network: acb_default_network, driver: 'bridge'
..../03/19 19:01:53 Successfully set up Docker network: acb_default_network
..../03/19 19:01:53 Setting up Docker configuration...
..../03/19 19:01:54 Successfully set up Docker configuration
..../03/19 19:01:54 Logging in to registry: mycontainerregistry008.azurecr.io
..../03/19 19:01:55 Successfully logged into mycontainerregistry008.azurecr.io
..../03/19 19:01:55 Executing step ID: acb_step_0. Working directory: '', Network: 'acb_default_network'
..../03/19 19:01:55 Launching container with name: acb_step_0

Hello from Docker!
This message shows that your installation appears to be working correctly.

..../03/19 19:01:56 Successfully executed container: acb_step_0
..../03/19 19:01:56 Step ID: acb_step_0 marked as successful (elapsed time in seconds: 0.843801)

Run ID: cab was successful after 6s
```

Clean up resources
Now that you have finished the project, you should delete the cloud resources you created to avoid unnecessary resource usage.

In your browser, navigate to the Azure portal https://portal.azure.com, signing in with your Azure credentials if prompted.
Navigate to the resource group you created and view the contents of the resources used in this exercise.
On the toolbar, select Delete resource group.
Enter the resource group name and confirm that you want to delete it.


CAUTION: Deleting a resource group deletes all resources contained within it. If you chose an existing resource group for this exercise, any existing resources outside the scope of this exercise will also be deleted.


Congratulations
successfully 
