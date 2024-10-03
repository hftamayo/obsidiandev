
Yes, you can automate the entire process using a shell script. This script will perform all the steps you outlined, including creating the resource group, service principal, role assignment, and secrets in Azure Key Vault. Here's a comprehensive script to automate these tasks:

### Shell Script to Automate Deployment Setup

```sh
#!/bin/bash

# Set variables
RESOURCE_GROUP="laserantsgroup"
LOCATION="westus"
KEYVAULT_NAME="laserantskeyvaul"
SP_NAME="laserants-shoppingcart-app"

# Step 1: Create a variable with your SUBSCRIPTION ID
SUBSCRIPTION_ID=$(az account show --query id --output tsv)
echo "Subscription ID: $SUBSCRIPTION_ID"

# Step 2: Create the resource group
az group create -n $RESOURCE_GROUP -l $LOCATION

# Step 3: Create the service principal and save the output
SP_OUTPUT=$(az ad sp create-for-rbac --name "$SP_NAME" --role contributor --scopes /subscriptions/$SUBSCRIPTION_ID/resourceGroups/$RESOURCE_GROUP --sdk-auth)
echo "Service Principal Output: $SP_OUTPUT"

# Step 4: Create a GitHub Actions secret called AZURE_CREDENTIALS with the output of the above command
# Note: This step requires GitHub CLI (gh) to be installed and authenticated

gh secret set AZURE_CREDENTIALS --body "$SP_OUTPUT"

# Step 5: Verify if the service principal was created
az ad sp list --display-name "$SP_NAME" --output table

# Extract the service principal ID from the output
SP_ID=$(echo $SP_OUTPUT | jq -r '.clientId')
echo "Service Principal ID: $SP_ID"

# Step 6: Create a Key Vault Contributor role for the service principal
az role assignment delete --assignee $SP_ID --role "Key Vault Contributor" --scope /subscriptions/$SUBSCRIPTION_ID/resourceGroups/$RESOURCE_GROUP/providers/Microsoft.KeyVault/vaults/$KEYVAULT_NAME
az role assignment create --assignee $SP_ID --role "Key Vault Contributor" --scope /subscriptions/$SUBSCRIPTION_ID/resourceGroups/$RESOURCE_GROUP/providers/Microsoft.KeyVault/vaults/$KEYVAULT_NAME

# Step 7: Create the secrets in Azure Key Vault
az keyvault secret set --vault-name $KEYVAULT_NAME --name backendport --value "8012"
az keyvault secret set --vault-name $KEYVAULT_NAME --name postgresuser --value "laserants"
az keyvault secret set --vault-name $KEYVAULT_NAME --name postgrespassword --value "laserants"
az keyvault secret set --vault-name $KEYVAULT_NAME --name postgresdb --value "ecomdev"
az keyvault secret set --vault-name $KEYVAULT_NAME --name postgreshost --value "pgecommerce"
az keyvault secret set --vault-name $KEYVAULT_NAME --name postgresport --value "5432"
az keyvault secret set --vault-name $KEYVAULT_NAME --name execmode --value "development"
az keyvault secret set --vault-name $KEYVAULT_NAME --name seeddevelopment --value "true"
az keyvault secret set --vault-name $KEYVAULT_NAME --name seedproduction --value "true"
az keyvault secret set --vault-name $KEYVAULT_NAME --name frontendorigins --value "http://localhost:5173"
az keyvault secret set --vault-name $KEYVAULT_NAME --name postgresdbdevurl --value "postgresql://laserants:laserants@pgecommerce:5432/ecomdev"
az keyvault secret set --vault-name $KEYVAULT_NAME --name databaseurl --value "postgresql://laserants:laserants@pgecommerce:5432/ecomdev"
az keyvault secret set --vault-name $KEYVAULT_NAME --name vitebackendurl --value "http://localhost:8012"
az keyvault secret set --vault-name $KEYVAULT_NAME --name viteappname --value "MORGAN THE PIRATE: BUY AND SELL BAY"

echo "All steps completed successfully."
```

### Notes

1. **GitHub CLI (gh)**: This script uses the GitHub CLI (`gh`) to set the GitHub Actions secret. Ensure you have the GitHub CLI installed and authenticated.
2. **jq**: This script uses `jq` to parse JSON. Ensure you have `jq` installed.
3. **Role Assignment Propagation**: Role assignments can take a few minutes to propagate. You might need to add a delay or retry logic if you encounter permission issues immediately after assigning roles.

### Running the Script

Save the script to a file, for example, `setup-deployment.sh`, and make it executable:

```sh
chmod +x setup-deployment.sh
```

Then run the script:

```sh
./setup-deployment.sh
```

By using this script, you can automate the entire setup process required before running your GitHub Actions workflow. Let me know if you need further assistance.

## pensando en automatizar todo el proceso:

To create a CI/CD pipeline that runs the shell script and then the GitHub Actions workflow, you have several options. Each tool has its strengths and can be chosen based on your specific requirements and existing infrastructure. Here are some options to consider:

### 1. GitHub Actions

**Pros**:
- Integrated with GitHub, making it easy to set up and manage.
- Supports running shell scripts and workflows in the same pipeline.
- No additional tools or services required.

**Cons**:
- Limited to GitHub repositories.

**Example**:
You can create a separate GitHub Actions workflow to run the shell script and then trigger the existing workflow.

### 2. Jenkins

**Pros**:
- Highly customizable and extensible with plugins.
- Can be integrated with various version control systems and build tools.
- Suitable for complex CI/CD pipelines.

**Cons**:
- Requires setup and maintenance of Jenkins server.
- More complex to configure compared to GitHub Actions.

**Example**:
You can create a Jenkins pipeline to run the shell script and then trigger the GitHub Actions workflow using the GitHub API.

### 3. Travis CI

**Pros**:
- Easy to set up and use with GitHub repositories.
- Supports running shell scripts and workflows.

**Cons**:
- Limited free tier for open-source projects.
- Less customizable compared to Jenkins.

**Example**:
You can create a `.travis.yml` file to run the shell script and then trigger the GitHub Actions workflow.

### 4. Terraform

**Pros**:
- Infrastructure as Code (IaC) tool, suitable for managing cloud resources.
- Can be used to set up the entire infrastructure, including resource groups, Key Vault, and role assignments.

**Cons**:
- Primarily focused on infrastructure management, not CI/CD pipelines.
- Requires learning Terraform syntax and concepts.

**Example**:
You can use Terraform to set up the Azure infrastructure and then use another CI/CD tool to run the shell script and GitHub Actions workflow.

### Recommended Approach: GitHub Actions

Given that your existing workflow is already in GitHub Actions, it makes sense to use another GitHub Actions workflow to run the shell script and then trigger the existing workflow. This approach keeps everything within the GitHub ecosystem and simplifies management.

### Example GitHub Actions Workflow

Here is an example of a GitHub Actions workflow that runs the shell script and then triggers the existing workflow:

```yaml
name: Setup and Deploy

on:
  push:
    branches:
      - main

jobs:
  setup:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Run setup script
      run: |
        chmod +x setup-deployment.sh
        ./setup-deployment.sh

    - name: Trigger deploy-to-aci workflow
      uses: actions/github-script@v4
      with:
        script: |
          const { Octokit } = require("@octokit/rest");
          const octokit = new Octokit({ auth: process.env.GITHUB_TOKEN });
          await octokit.actions.createWorkflowDispatch({
            owner: context.repo.owner,
            repo: context.repo.repo,
            workflow_id: "deploy-to-aci.yml",
            ref: "main"
          });
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### Summary

- **GitHub Actions**: Recommended for simplicity and integration with your existing workflow.
- **Jenkins**: Suitable for complex pipelines and extensive customization.
- **Travis CI**: Easy to set up, good for GitHub repositories.
- **Terraform**: Best for infrastructure management, not CI/CD pipelines.

By using GitHub Actions, you can automate the entire setup and deployment process within the GitHub ecosystem, making it easier to manage and maintain. Let me know if you need further assistance.

## Destroy everything

Creating a GitHub Actions workflow to delete the Azure resource group is a good idea for automating the cleanup process. This ensures that you can easily tear down the infrastructure when it's no longer needed.

### GitHub Actions Workflow for Cleanup

Here is an example of a GitHub Actions workflow that deletes the Azure resource group:

```yaml
name: Cleanup Azure Resources

on:
  workflow_dispatch:

jobs:
  cleanup:
    runs-on: ubuntu-latest

    steps:
    - name: Set up Azure CLI
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}

    - name: Delete Azure Resource Group
      run: az group delete -n laserantsgroup --yes --no-wait
```

### Explanation

- **Trigger**: The workflow is triggered manually using `workflow_dispatch`. This allows you to run the cleanup process on demand.
- **Set up Azure CLI**: Logs in to Azure using the credentials stored in the `AZURE_CREDENTIALS` secret.
- **Delete Resource Group**: Executes the `az group delete` command to delete the specified resource group.

### Additional Considerations

1. **Manual Trigger**: The `workflow_dispatch` trigger allows you to manually trigger the cleanup workflow from the GitHub Actions tab in your repository.
2. **Environment Protection**: Ensure that only authorized users can trigger the cleanup workflow to prevent accidental deletion of resources.
3. **Resource Dependencies**: Be aware of any dependencies between resources. Deleting the resource group will delete all resources within it.

### Running the Cleanup Workflow

To run the cleanup workflow:

1. Go to the "Actions" tab in your GitHub repository.
2. Select the "Cleanup Azure Resources" workflow.
3. Click the "Run workflow" button.

By setting up this GitHub Actions workflow, you can automate the cleanup process and ensure that your Azure resources are properly managed. Let me know if you need further assistance.