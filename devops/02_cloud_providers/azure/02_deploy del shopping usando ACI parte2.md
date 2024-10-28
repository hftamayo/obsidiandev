El status es el siguiente:
- tengo los roles asignados de Key Vault Contributor y Key Vault Secrets Officer
- aun asi no puedo crear secrets en el vault
- los containers siguen deployados pero sin funcionar porque hasta no tener los secrets seteados no puedo correr una actualizacion

```

Comandos para chequear permisos de los roles:
- az role definition list --name "Key Vault Contributor"
- az role definition list --name "Key Vault Secrets Officer"

Verificando que no hay ningun deny por default que bloquee el acceso de la cuenta al vault:

- az role assignment list --assignee dc542e2a-af9c-47d4-8bc8-fcb1f54dddce --all --include-deny-assignment

Chequeando roles asignados:
- az role assignment list --assignee dc542e2a-af9c-47d4-8bc8-fcb1f54dddce --all

verificando que el appID y el oID hagan match con el SP:
- az ad sp show --id b677c290-cf4b-4a8e-a60e-91ba650a4abe

```

Justo en este ultimo comando generó un error:

![[Pasted image 20241002104512.png]]

### Sugerencia de Copilot:

Thank you for providing the detailed outputs. It appears that the Service Principal (`dc542e2a-af9c-47d4-8bc8-fcb1f54dddce`) has the necessary roles assigned, but the error message indicates that the caller's `appid` and `oid` do not match this Service Principal.

### Steps to Resolve the Issue

1. **Verify the Correct Service Principal**:
   - Ensure that the Service Principal you are using to run the `az keyvault secret set` command is the same one that has the roles assigned.

2. **Check the Azure CLI Login**:
   - Ensure that you are logged in with the correct Service Principal. You can log in using the Service Principal credentials.

### Log in with the Correct Service Principal

1. **Log Out of the Current Session**:
   ```sh
   az logout
   ```

2. **Log In with the Service Principal**:
   - Use the Service Principal's `AppId`, `TenantId`, and `Password` to log in.
   ```sh
   az login --service-principal --username dc542e2a-af9c-47d4-8bc8-fcb1f54dddce --password <YOUR_SP_PASSWORD> --tenant <YOUR_TENANT_ID>
   ```

3. **Verify the Login**:
   - Ensure that you are logged in with the correct Service Principal.
   ```sh
   az account show
   ```

### Test the Command Again

After logging in with the correct Service Principal, try setting the secret again:

```sh
az keyvault secret set --vault-name laserantskeyvaul --name backendport --value "8012"
```

### Summary

- **Verify the Correct Service Principal**: Ensure the Service Principal being used has the necessary roles.
- **Log In with the Correct Service Principal**: Use the Service Principal credentials to log in.
- **Test the Command Again**: Try setting the secret after logging in with the correct Service Principal.

By following these steps, you should be able to resolve the authorization issue and successfully set the secret in your Key Vault.

![[Pasted image 20241002104740.png]]

The output of `az account show` indicates that you are currently logged in as a user (`herbert.fernandez@ues.edu.sv`) rather than the Service Principal. This is why the `appid` and `oid` in the error message do not match the Service Principal that has the necessary roles assigned.

To resolve this, you need to log in using the Service Principal credentials. Here are the steps:

### ==Steps to Log In with the Service Principal

1. **Log Out of the Current Session**:
   ```sh
   az logout
   ```

2. **Log In with the Service Principal**:
   - Use the Service Principal's `AppId`, `TenantId`, and `Password` to log in. Replace `<YOUR_SP_PASSWORD>` and `<YOUR_TENANT_ID>` with the actual values.
   ```sh
   az login --service-principal --username dc542e2a-af9c-47d4-8bc8-fcb1f54dddce --password <YOUR_SP_PASSWORD> --tenant de44d3c8-30dc-416f-b26c-4e533c37353f
   ```

3. **Verify the Login**:
   - Ensure that you are logged in with the correct Service Principal.
   ```sh
   az account show
   ```

   The output should show the `user` field with the `type` as `servicePrincipal`.

### Test the Command Again

After logging in with the correct Service Principal, try setting the secret again:

```sh
az keyvault secret set --vault-name laserantskeyvaul --name backendport --value "8012"
```

### Summary

- **Log Out of the Current Session**: Ensure you are logged out of the current user session.
- **Log In with the Service Principal**: Use the Service Principal credentials to log in.
- **Verify the Login**: Ensure the login is successful and the `user` type is `servicePrincipal`.
- **Test the Command Again**: Try setting the secret after logging in with the correct Service Principal.

By following these steps, you should be able to resolve the authorization issue and successfully set the secret in your Key Vault.

To log in with a Service Principal, you need the following information:
1. **AppId** (Client ID) of the Service Principal.
2. **Password** (Client Secret) for the Service Principal.
3. **TenantId** (Directory ID) of your Azure AD tenant.

### ==Steps to Obtain the Required Information

#### 1. **Obtain the Tenant ID**

You can obtain the Tenant ID using the Azure CLI:

```sh
az account show --query tenantId --output tsv
```

![[Pasted image 20241002105356.png]]

```
herbert_fernandez [ ~ ]$ az account show --query tenantId --output tsv
de44d3c8-30dc-416f-b26c-4e533c37353f

```

#### 2. **Create a Client Secret for the Service Principal**

If you haven't set a password (client secret) for the Service Principal, you can create one:

```sh
az ad sp credential reset --id <sp object id>
```

![[Pasted image 20241002105942.png]]

```bash
{
  "appId": "dc542e2a-af9c-47d4-8bc8-fcb1f54dddce",
  "buffo": "",
  "tenant": "de44d3c8-30dc-416f-b26c-4e533c37353f"
}
```



This command will output a new client secret. Make sure to save this secret securely, as you will need it to log in.

### ==tratando de logearse como SP sin cerrar mi sesion actual:

Azure CLI does not have a direct equivalent to the `su` command in Linux for switching users within the same session. However, you can use the Azure CLI to log in as a Service Principal in a separate session or context without affecting your current user session.

### Options to Log In as a Service Principal Without Logging Out

1. **Use Azure CLI Profiles**:
   - Azure CLI supports multiple profiles, allowing you to switch between different accounts or Service Principals without logging out of your current session. You can use the `--profile` option to specify a different profile.

2. **Use Environment Variables**:
   - You can set environment variables to temporarily use the Service Principal credentials for specific commands.

3. **Use Azure CLI Contexts**:
   - Azure CLI contexts allow you to manage multiple sets of credentials and switch between them.

### Steps to Use Azure CLI Profiles

1. **Create a New Profile for the Service Principal**:
   - First, create a new profile for the Service Principal. This will allow you to switch between your user account and the Service Principal without logging out.

2. **Log In with the Service Principal**:
   - Use the Service Principal's `AppId`, `TenantId`, and `Client Secret` to log in and save the credentials to the new profile.

### Example Commands

1. **Create a New Profile**:
   - Set the `AZURE_CONFIG_DIR` environment variable to a new directory for the Service Principal profile.
   ```sh
   export AZURE_CONFIG_DIR=~/.azure/sp-profile
   ```

2. **Log In with the Service Principal**:
   - Use the Service Principal's `AppId`, `TenantId`, and `Client Secret` to log in.
   ```sh
   az login --service-principal --username <SP_APP_ID> --password <SP_CLIENT_SECRET> --tenant <TENANT_ID>
   ```


![[Pasted image 20241002111155.png]]

```bash

- az ad sp credential reset --id 08750a78-79ca-4e39-9ffc-69e3b3d50b12

{
  "appId": "dc542e2a-af9c-47d4-8bc8-fcb1f54dddce",
  "buffo": "XXXX",
  "tenant": "de44d3c8-30dc-416f-b26c-4e533c37353f"
}

- export AZURE_CONFIG_DIR=~/.azure/laserants-sp-profile
- az login --service-principal --username dc542e2a-af9c-47d4-8bc8-fcb1f54dddce --buffo XXX --tenant de44d3c8-30dc-416f-b26c-4e533c37353f

[
  {
    "cloudName": "AzureCloud",
    "homeTenantId": "de44d3c8-30dc-416f-b26c-4e533c37353f",
    "id": "e8698c35-8189-4f9a-be17-a00cfa7f6c02",
    "isDefault": true,
    "managedByTenants": [],
    "name": "Azure for Students",
    "state": "Enabled",
    "tenantId": "de44d3c8-30dc-416f-b26c-4e533c37353f",
    "user": {
      "name": "dc542e2a-af9c-47d4-8bc8-fcb1f54dddce",
      "type": "servicePrincipal"
    }
  }
]

- az account show

```


3. **Verify the Login**:
   - Ensure that you are logged in with the Service Principal.
   ```sh
   az account show
   ```

![[Pasted image 20241002111432.png]]

### Switch Between Profiles

To switch back to your original user profile, unset the `AZURE_CONFIG_DIR` environment variable:

```sh
unset AZURE_CONFIG_DIR
```

To switch to the Service Principal profile again, set the `AZURE_CONFIG_DIR` environment variable:

```sh
export AZURE_CONFIG_DIR=~/.azure/sp-profile
```

### Summary

- **Create a New Profile**: Use the `AZURE_CONFIG_DIR` environment variable to create a new profile for the Service Principal.
- **Log In with the Service Principal**: Use the Service Principal credentials to log in and save the credentials to the new profile.
- **Switch Between Profiles**: Use the `AZURE_CONFIG_DIR` environment variable to switch between your user profile and the Service Principal profile.

By following these steps, you can manage multiple profiles and switch between your user account and the Service Principal without logging out of your current session.

No, the profile will not be destroyed when you close the Azure terminal. The profile is stored in the directory specified by the `AZURE_CONFIG_DIR` environment variable, and it will persist across terminal sessions.

### How Profiles Work

- **Profile Directory**: The profile is stored in the directory you specify with `AZURE_CONFIG_DIR`. This directory contains the configuration and credentials for the profile.
- **Persistence**: The profile data is saved on disk, so it will remain available even after you close the terminal or restart your machine.
- **Switching Profiles**: You can switch between profiles by setting the `AZURE_CONFIG_DIR` environment variable to different directories.

### Example Workflow

1. **Create and Use a Profile**:
   - Set the `AZURE_CONFIG_DIR` to a new directory and log in as the Service Principal.
   ```sh
   export AZURE_CONFIG_DIR=~/.azure/sp-profile
   az login --service-principal --username <SP_APP_ID> --password <SP_CLIENT_SECRET> --tenant <TENANT_ID>
   ```

2. **Switch Back to the Default Profile**:
   - Unset the `AZURE_CONFIG_DIR` to switch back to the default profile.
   ```sh
   unset AZURE_CONFIG_DIR
   ```

3. **Switch to the Service Principal Profile Again**:
   - Set the `AZURE_CONFIG_DIR` to the Service Principal profile directory.
   ```sh
   export AZURE_CONFIG_DIR=~/.azure/sp-profile
   ```

### Summary

- **Profiles Persist**: The profiles are stored on disk and will persist across terminal sessions.
- **Switching Profiles**: You can switch between profiles by setting the `AZURE_CONFIG_DIR` environment variable to different directories.
- **Profile Management**: This allows you to manage multiple profiles and switch between them as needed without losing any configuration or credentials.

By using this method, you can maintain multiple profiles and switch between them easily, ensuring that your configurations and credentials are preserved.

### probando si puedo setear Secrets:

![[Pasted image 20241002112522.png]]

==efectivametne aca si pude setear el secret backendport y asi fue los demás

### asegurando que el SP tenga acceso al Key Vault:

Si estoy usando un SP que no tiene activa el RBAC puede usarse este comando:

```
az keyvault set-policy --name <your-keyvault-name> --spn <your-service-principal-id> --secret-permissions get

```

Sin embargo, en mi caso si tengo activo el RBAC asi que tengo que hacer la ejecución de estos pasos:

```
unset AZURE_CONFIG_DIR
az account show

az role assignment create --role "Key Vault Secrets User" --assignee dc542e2a-af9c-47d4-8bc8-fcb1f54dddce --scope /subscriptions/$SUBSCRIPTION_ID/resourceGroups/laserantsgroup/providers/Microsoft.KeyVault/vaults/laserantskeyvaul

```

![[Pasted image 20241002115718.png]]

![[Pasted image 20241002115833.png]]

verificando si tengo los roles asignados:

```bash
az role assignment list --assignee $SP_ID --scope /subscriptions/$SUBSCRIPTION_ID/resourceGroups/laserantsgroup/providers/Microsoft.KeyVault/vaults/laserantskeyvaul | grep "roleDefinitionName"

```

![[Pasted image 20241002120314.png]]

pues ya en este momento estoy listo para probar mi action y ver que ondas:

## Resultados:

lo más importante es que durante el deploy del action ==en la fase de contenedores estoy teniendo tiempos altisimos de deploy a grado que tengo que detener el proceso y es aca donde al consultar con Copilot encontré el termine green-blue deployment:==

- name: Deploy new version of pgecommerce to ACI
  run: |
    az container create \
      --resource-group laserantsgroup \
      --name pgecommerce-v2 \
      --image hftamayo/pgecommerce:0.0.1 \
      --cpu 1 \
      --memory 1 \
      --ports 5432 \
      --registry-login-server index.docker.io \
      --registry-username ${{ secrets.DOCKER_HUB_USERNAME }} \
      --registry-password ${{ secrets.DOCKER_HUB_PAT }} \
      --vnet laserantsvnet \
      --subnet lasersubnet \
      --ip-address Private \
      --environment-variables POSTGRES_USER=$POSTGRES_USER \
      POSTGRES_PASSWORD=$POSTGRES_PASSWORD \
      POSTGRES_DB=$POSTGRES_DB \
      POSTGRES_HOST=$POSTGRES_HOST \
      POSTGRES_PORT=$POSTGRES_PORT \
      EXEC_MODE=$EXEC_MODE \
      DATABASE_URL=$DATABASE_URL

- name: Switch traffic to new container
  run: |
    # Add commands to update DNS or load balancer to point to the new container
    # Example: az network dns record-set a add-record ...

- name: Delete old container
  run: |
    az container delete --resource-group laserantsgroup --name pgecommerce --yes

==en esta etapa los containers esta en on pero no hacen nada asi que voy a borrarlos pero en un futuro el action tiene que incluir el deployment blue-green==

## Rutinas de Health Check

### utilidad de las rutinas health check

The routine you're referring to is creating a custom health probe for an Azure Application Gateway. A health probe is used to monitor the health of backend resources (e.g., VMs, containers) and ensure that traffic is only routed to healthy instances.

### Purpose of the Health Probe Routine

1. **Health Monitoring**: The health probe periodically checks the health of the backend resources by sending requests to a specified endpoint.
2. **Traffic Routing**: Based on the health probe results, the Application Gateway can route traffic only to healthy backend instances, improving the reliability and availability of your application.
3. **Custom Configuration**: Custom health probes allow you to specify the protocol, path, interval, timeout, and other settings to tailor the health checks to your application's needs.

### Is It Strictly Necessary?

- **Production Environment**: In a production environment, health probes are highly recommended to ensure that traffic is only routed to healthy instances. This helps maintain high availability and reliability.
- **Development/Testing**: In a development or testing environment, you might not need custom health probes, but it's still a good practice to include them to mimic production conditions.

### Summary

- **Purpose**: The health probe routine is used to monitor the health of backend resources and ensure traffic is routed only to healthy instances.
- **Necessity**: In a production-ready environment, including health probes is highly recommended to maintain high availability and reliability. In development or testing environments, it might not be strictly necessary but is still a good practice.

Let me know if you need further assistance or if you would like to proceed with fixing the issue in the routine.


### poner en pausa las rutinas de health check:

Understood. It's important to have a clear understanding of your architecture and monitoring requirements before implementing custom health probes. Here are some considerations and steps you can take to make an informed decision:

### Considerations for Health Probes

1. **Identify Critical Services**:
   - Determine which services or components are critical to your application's functionality and need to be monitored.
   - For a monorepo, consider splitting the backend and frontend services if they have different health check requirements.

2. **Define Health Check Endpoints**:
   - Each service should expose a health check endpoint (e.g., `/health`) that the health probe can use to determine the service's status.

3. **Determine Health Probe Configuration**:
   - Decide on the appropriate configuration for the health probes, such as the protocol (HTTP/HTTPS), path, interval, timeout, and host.

4. **Integration with Application Gateway**:
   - Ensure that the health probes are correctly integrated with the Azure Application Gateway to route traffic only to healthy instances.

### Steps to Proceed

1. **Review Architecture**:
   - Review your application's architecture to identify the services that need to be monitored.
   - Consider splitting the monorepo into separate backend and frontend services if necessary.

2. **Define Health Check Endpoints**:
   - Ensure that each service exposes a health check endpoint that can be used by the health probes.

3. **Plan Health Probe Configuration**:
   - Plan the configuration for the health probes, including the protocol, path, interval, timeout, and host.

4. **Implement and Test**:
   - Implement the health probes and test them to ensure they correctly monitor the health of your services.

### Summary

- **Identify Critical Services**: Determine which services need to be monitored.
- **Define Health Check Endpoints**: Ensure each service has a health check endpoint.
- **Plan Health Probe Configuration**: Decide on the configuration for the health probes.
- **Implement and Test**: Implement the health probes and test them.

By following these steps, you can ensure that your health probes are correctly configured and effectively monitor the health of your services. Let me know if you need further assistance or if you have any specific questions.


## problemas de comunicacion

1. ambos containers se pudieron deployar pasandoles los secrets, ambos tienen IP Address
2. pgecommerce esta arriba
3. estando en el bash puedo pinear al uno del otro
4. las ips estan en el mismo rango
5. los containers son deployados en una misma red virtual
6. por qué no pueden resolverse por nombre?

==The issue likely stems from the fact that Azure Container Instances (ACI) does not provide built-in DNS resolution for container names within a virtual network. Containers in ACI can communicate using their private IP addresses, but they cannot resolve each other by name out of the box.

### Solution: Use Private IP Addresses

Since you've already deployed both containers to the same virtual network and subnet, you can use the private IP address of `pgecommerce` in the environment variables for [`ecommerce-monorepo`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A79%2C%22character%22%3A17%7D%7D%5D%2C%22d8c04750-92fb-4d3c-9b0e-e32846da52ba%22%5D "Go to definition").

### Steps to Implement

1. **Deploy `pgecommerce` and Retrieve its IP Address**:
   - Deploy `pgecommerce` and retrieve its private IP address.
2. **Update Environment Variables for [`ecommerce-monorepo`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A79%2C%22character%22%3A17%7D%7D%5D%2C%22d8c04750-92fb-4d3c-9b0e-e32846da52ba%22%5D "Go to definition")**:
   - Use the retrieved IP address in the environment variables for [`ecommerce-monorepo`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A79%2C%22character%22%3A17%7D%7D%5D%2C%22d8c04750-92fb-4d3c-9b0e-e32846da52ba%22%5D "Go to definition").
3. **Deploy [`ecommerce-monorepo`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A79%2C%22character%22%3A17%7D%7D%5D%2C%22d8c04750-92fb-4d3c-9b0e-e32846da52ba%22%5D "Go to definition")**:
   - Deploy [`ecommerce-monorepo`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A79%2C%22character%22%3A17%7D%7D%5D%2C%22d8c04750-92fb-4d3c-9b0e-e32846da52ba%22%5D "Go to definition") with the updated environment variables.

### Updated Workflow

Here is the updated workflow to achieve this:

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v3

    - name: Log in to Docker Hub
      run: echo "${{ secrets.DOCKER_HUB_PAT }}" | docker login -u "${{ secrets.DOCKER_HUB_USERNAME }}" --password-stdin

    - name: Set up Azure CLI
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}

    - name: Deploy pgecommerce to ACI
      run: |
        az container create \
          --resource-group laserantsgroup \
          --name pgecommerce \
          --image hftamayo/pgecommerce:0.0.1 \
          --cpu 1 \
          --memory 1 \
          --ports 5432 \
          --registry-login-server index.docker.io \
          --registry-username ${{ secrets.DOCKER_HUB_USERNAME }} \
          --registry-password ${{ secrets.DOCKER_HUB_PAT }} \
          --vnet laserantsvnet \
          --subnet lasersubnet \
          --ip-address Private \
          --environment-variables ${{ env.DATALAYER_VARS }}

    - name: Retrieve IP address of pgecommerce
      id: get_pgecommerce_ip
      run: |
        PGE_COMMERCE_IP=$(az container show --resource-group laserantsgroup --name pgecommerce --query ipAddress.ip --output tsv)
        echo "PGE_COMMERCE_IP=$PGE_COMMERCE_IP" >> $GITHUB_ENV

    - name: Deploy ecommerce_monorepo to ACI
      run: |
        az container create \
          --resource-group laserantsgroup \
          --name ecommerce-monorepo \
          --image hftamayo/ecommerce_monorepo:0.0.1 \
          --cpu 1 \
          --memory 1 \
          --ports 5173 8012 \
          --registry-login-server index.docker.io \
          --registry-username ${{ secrets.DOCKER_HUB_USERNAME }} \
          --registry-password ${{ secrets.DOCKER_HUB_PAT }} \
          --vnet laserantsvnet \
          --subnet lasersubnet \
          --ip-address Private \
          --environment-variables DATABASE_URL=postgresql://laserants:laserants@${{ env.PGE_COMMERCE_IP }}:5432/ecomdev
```

### Summary

- **Deploy `pgecommerce`**: Deploy the `pgecommerce` container and retrieve its private IP address.
- **Update Environment Variables**: Use the retrieved IP address in the environment variables for [`ecommerce-monorepo`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A79%2C%22character%22%3A17%7D%7D%5D%2C%22d8c04750-92fb-4d3c-9b0e-e32846da52ba%22%5D "Go to definition").
- **Deploy [`ecommerce-monorepo`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A79%2C%22character%22%3A17%7D%7D%5D%2C%22d8c04750-92fb-4d3c-9b0e-e32846da52ba%22%5D "Go to definition")**: Deploy the [`ecommerce-monorepo`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A79%2C%22character%22%3A17%7D%7D%5D%2C%22d8c04750-92fb-4d3c-9b0e-e32846da52ba%22%5D "Go to definition") container with the updated environment variables.

This approach ensures that [`ecommerce-monorepo`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A79%2C%22character%22%3A17%7D%7D%5D%2C%22d8c04750-92fb-4d3c-9b0e-e32846da52ba%22%5D "Go to definition") can connect to `pgecommerce` using its private IP address. Let me know if you need further assistance.


SUBSCRIPTION_ID=$(az account list --query "[?isDefault].id" -o tsv)
az group create --name vanillatstodogroup --location westus

az ad sp create-for-rbac --name vanillatstodosp --role contributor --scopes /subscriptions/$SUBSCRIPTION_ID/resourceGroups/vanillatstodogroup --sdk-auth

az aks delete --name vanillatstodocluster --resource-group vanillatstodogroup --yes --no-wait

az aks show --name vanillatstodocluster --resource-group vanillatstodogroup

az ad sp list --display-name vanillatstodosp --query "[].{id:appId, name:displayName}"

az role assignment list --assignee fab62c18-fdc3-4140-a059-02392afe7a7f --scope /subscriptions/$SUBSCRIPTION_ID/resourceGroups/vanill
atstodogroup

az group delete --name vanillatstodogroup --yes --no-wait
