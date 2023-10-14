# How to create a GitHub Actions workflow that builds and deploys a machine learning model to Azure Machine Learning. 

# Pre-Requisites:

1. Azure Machine Learning WorkSpace - Create a workspace [Create Workspace](https://learn.microsoft.com/en-us/azure/machine-learning/quickstart-create-resources?view=azureml-api-2), if you don't have one.
2. Python SDK V2 - `(pip install azure-ai-ml azure-identity)`
3. Github Account.

# Authenticate with Azure

- Create a Service Principle with Azure CLI. 
- Run the Following command in Azure Cloud Shell:
`(az ad sp create-for-rbac --name "myML" --role contributor --scopes /subscriptions/<subscription-id>/resourceGroups/<group-name> --sdk-auth)`

- The output is a JSON object with the role assignment credentials that provide access to your App Service app. Copy the Json Object for later use.

# Create Secrets

- Open your Github, Navigate to settings , Security > Secrets and Variables > Actions.
- Select New Repository Secret.
- Paste the JSON Output from Azure CLI in the Secrets value field. Give the secrets name like `(AZURE_CREDENTIALS)`.
- Add Secret.

# Update `(setup.sh)` to connect to your Azure ML WorkSpace

- You'll need to update the CLI setup file variables to match your workspace.
- In setup.sh update the Following (GROUP, LOCATION, WORKSPACE).

#  Update `(pipeline.yml)` with your compute cluster name

- You'll use a pipeline.yml file to deploy your Azure Machine Learning pipeline. This is a machine learning pipeline and not a DevOps pipeline. You only need to make this update if you're using a name other than cpu-cluster for your computer cluster name

- Each time you see compute: azureml:cpu-cluster, update the value of cpu-cluster with your compute cluster name. For example, if your cluster is named my-cluster, your new value would be azureml:my-cluster. 

# Run Your Github Actions workflow

Your workflow file is made up of a trigger section and jobs:

- A trigger starts the workflow in the on section. The workflow runs by default on a cron schedule and when a pull request is made from matching branches and paths.

- In the jobs section of the workflow, you checkout code and log into Azure with your service principal secret.
- Select View runs.
- Enable workflows by selecting I understand my workflows, go ahead and enable them.
- Select the cli-jobs-pipelines-<your-name> workflow and choose to Enable workflow.
- Select Run workflow and choose the option to Run workflow now.

# Verify your WorkFlow run

- Open your completed workflow run and verify that the build job ran successfully. You'll see a green checkmark next to the job.

- Open Azure Machine Learning studio and navigate to the pipeline-example. Verify that each part of your job (prep, transform, train, predict, score) completed and that you see a green checkmark.