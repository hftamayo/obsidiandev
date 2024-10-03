en esencia en este metodo unicamente necesito el service principal, en esta imagen se lista que tengo ademas del SP el group y el registry:
![[Pasted image 20240916113636.png]]

verificar que la subscripcion este registrado con ACS:
```
az provider register --namespace Microsoft.ContainerInstance

az provider show --namespace Microsoft.ContainerInstance --query "registrationState"

Crear una subred:

az network vnet create --resource-group laserantsgroup --name laserantsvnet --address-prefix 10.0.0.0/16 --subnet-name lasersubnet --subnet-prefix 10.0.0.0/24

```

![[Pasted image 20240916131918.png]]


==y del lado de Azure eso es todo, ahorita tengo que crear el secret del AZURE_CREDENTIAL de este SP:

![[Pasted image 20240916120855.png]]

PR para deployar el action a Unstable branch:

![[Pasted image 20240916120828.png]]

after de un par de ajustes:

![[Pasted image 20240916124642.png]]

despues de algunos ajustes:
![[Pasted image 20240916130654.png]]


## INTERACCION ENTRE LOS CONTENEDORES EN BARE METAL:

### Data Layer: pgecommerce

- la imagen de la DB contiene además del runtime de pg, un script db.sql que crea la base de datos y el usuario
- es necesario que el contenedor de la aplicacion esten en la misma red para que puedan resolverse
- el container ==no necesita leer el env file del backend puesto que el script db.sql es ejecutado en el init process por la imagen, per se, esto puede significar un agujero de seguridad ==
- por tanto el container puede crearse asi:

```
docker run --name pgecommerce -p 5432:5432 hftamayo/pgecommerce:0.0.1

```

### Monorepo: 

- en esencia .env controla el host donde la DB layer debe encontrarse
- ambos contenedores deben estar en una misma red

```
En caso que olvidé vincultar algun container a la red
- docker start pgecommerce
- docker network connect ecommerce_network pgecommerce
- docker network inspect ecommerce_network

```

![[Pasted image 20240930145235.png]]

==mensaje cuando los containers no estan en la misma red o no se pueden alcanzar:

![[Pasted image 20240930145431.png]]

==mensaje cuando no pueden comunicarse porque hay un error en el .env:

![[Pasted image 20240930145516.png]]

![[Pasted image 20240930150231.png]]


subir las imagenes al docker hub

```
docker tag hftamayo/pgecommerce:0.0.1 hftamayo/pgecommerce:0.0.1
docker tag hftamayo/ecommerce_monorepo:0.0.1 hftamayo/ecommerce_monorepo:0.0.1

docker push hftamayo/pgecommerce:0.0.1
docker push hftamayo/ecommerce_monorepo:0.0.1

```


### CREANDO EL GRUPO Y EL SP EN AZURE PARA CORRER EL ACTION:

```bash

SUBSCRIPTION_ID=$(az account show --query id --output tsv)
echo $SUBSCRIPTION_ID

az group create -n laserantsgroup -l westus

az ad sp create-for-rbac --name "laserants-shoppingcart-app" --role contributor --scopes /subscriptions/$SUBSCRIPTION_ID/resourceGroups/laserantsgroup --sdk-auth

az ad sp list --display-name "laserants-shoppingcart-app" --output table

```


![[Pasted image 20241001111947.png]]

![[Pasted image 20241001112404.png]]

![[Pasted image 20241001112454.png]]

==la creacion de la subnet asi como los otros recursos estan delegados al github actions, una vez que el PR ha sido aprobado para la rama unstable el action se ejecuta:

![[Pasted image 20241001113951.png]]

## RESULTADOS Y DEBUG

Pues yo estaba con la clavazon de crear una subnet pues eso me funciono en bare metal, de hecho, durante el deploy cai en este rabbit hole:

```bash

ERROR: Can not use "--subnet" or "--vnet" with IP address type "Public".

[19](https://github.com/htues/laserants_tsbc_hw03/actions/runs/11130556440/job/30930150101#step:12:20)Error: Process completed with exit code 1.

```

==respuesta de Copilot:

The error you're encountering is due to the fact that Azure Container Instances (ACI) does not support using a public IP address (`--ip-address Public`) in combination with a virtual network ([`--vnet`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A67%2C%22character%22%3A10%7D%7D%5D%2C%22162d06d7-e1d9-4771-8fa8-07e67a94c3a2%22%5D "Go to definition") and [`--subnet`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A68%2C%22character%22%3A10%7D%7D%5D%2C%22162d06d7-e1d9-4771-8fa8-07e67a94c3a2%22%5D "Go to definition")). To resolve this, you have a couple of options:

### Option 1: Use a Private IP Address and Set Up a Public Load Balancer

You can deploy both containers with private IP addresses and then set up an Azure Load Balancer or Application Gateway to expose the [`ecommerce-monorepo`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A59%2C%22character%22%3A17%7D%7D%5D%2C%22162d06d7-e1d9-4771-8fa8-07e67a94c3a2%22%5D "Go to definition") container to the internet.

### Option 2: Deploy [`ecommerce-monorepo`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A59%2C%22character%22%3A17%7D%7D%5D%2C%22162d06d7-e1d9-4771-8fa8-07e67a94c3a2%22%5D "Go to definition") Without a Virtual Network

If the [`ecommerce-monorepo`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A59%2C%22character%22%3A17%7D%7D%5D%2C%22162d06d7-e1d9-4771-8fa8-07e67a94c3a2%22%5D "Go to definition") container does not need to communicate with other resources in the virtual network, you can deploy it without specifying the virtual network and subnet, allowing it to use a public IP address.

### Option 3: Use Azure Kubernetes Service (AKS)

For more complex networking requirements, consider using Azure Kubernetes Service (AKS), which provides more flexibility in networking configurations.

### Implementing Option 2

Since Option 2 is the simplest and quickest to implement, let's update your workflow to deploy the [`ecommerce-monorepo`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A59%2C%22character%22%3A17%7D%7D%5D%2C%22162d06d7-e1d9-4771-8fa8-07e67a94c3a2%22%5D "Go to definition") container without specifying the virtual network and subnet.

### Updated Workflow

Here is the updated GitHub Actions workflow:

```yaml
name: Deploy to ACI

on:
  push:
    branches:
      - unstable

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Log in to Docker Hub
      run: echo "${{ secrets.DOCKER_HUB_PAT }}" | docker login -u "${{ secrets.DOCKER_HUB_USERNAME }}" --password-stdin

    - name: Set up Azure CLI
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}

    - name: Ensure virtual network and subnet exist
      run: |
        if ! az network vnet show --resource-group laserantsgroup --name laserantsvnet; then
          az network vnet create --resource-group laserantsgroup --name laserantsvnet --address-prefix 10.0.0.0/16 --subnet-name lasersubnet --subnet-prefix 10.0.0.0/24
        fi

    - name: Ensure network security group exists
      run: |
        if ! az network nsg show --resource-group laserantsgroup --name laserantsnsg; then
          az network nsg create --resource-group laserantsgroup --name laserantsnsg --location westus
        fi

    - name: Ensure NSG rules allow internal communication
      run: |
        az network nsg rule create --resource-group laserantsgroup --nsg-name laserantsnsg --name AllowInternal --priority 100 --direction Inbound --access Allow --protocol Tcp --source-address-prefixes 10.0.0.0/16 --source-port-ranges '*' --destination-address-prefixes 10.0.0.0/16 --destination-port-ranges '*'

    - name: Associate NSG with subnet
      run: |
        az network vnet subnet update --resource-group laserantsgroup --vnet-name laserantsvnet --name lasersubnet --network-security-group laserantsnsg

    - name: Delete existing ecommerce-monorepo container group
      run: |
        if az container show --resource-group laserantsgroup --name ecommerce-monorepo; then
          az container delete --resource-group laserantsgroup --name ecommerce-monorepo --yes
        fi

    - name: Delete existing pgecommerce container group
      run: |
        if az container show --resource-group laserantsgroup --name pgecommerce; then
          az container delete --resource-group laserantsgroup --name pgecommerce --yes
        fi

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
          --ip-address Public \
          --environment-variables \
            DATABASE_URL=${{ secrets.DATABASE_URL }}

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
          --environment-variables \
            POSTGRES_DB=${{ secrets.POSTGRES_DB }} \
            POSTGRES_USER=${{ secrets.POSTGRES_USER }} \
            POSTGRES_PASSWORD=${{ secrets.POSTGRES_PASSWORD }}
```

### Summary

- **Deploy [`ecommerce-monorepo`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A59%2C%22character%22%3A17%7D%7D%5D%2C%22162d06d7-e1d9-4771-8fa8-07e67a94c3a2%22%5D "Go to definition") Without Virtual Network**: The [`ecommerce-monorepo`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A59%2C%22character%22%3A17%7D%7D%5D%2C%22162d06d7-e1d9-4771-8fa8-07e67a94c3a2%22%5D "Go to definition") container is deployed with a public IP address and without specifying the virtual network and subnet.
- **Deploy [`pgecommerce`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A51%2C%22character%22%3A68%7D%7D%5D%2C%22162d06d7-e1d9-4771-8fa8-07e67a94c3a2%22%5D "Go to definition") With Private IP**: The [`pgecommerce`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A51%2C%22character%22%3A68%7D%7D%5D%2C%22162d06d7-e1d9-4771-8fa8-07e67a94c3a2%22%5D "Go to definition") container is deployed with a private IP address within the specified virtual network and subnet.

By following this updated workflow, you can avoid the issue of combining public IP addresses with virtual networks in Azure Container Instances.

==ahora bien, la opcion anterior no va a permitir que ambos container se comunique puesto que estarán en segmentos de red distintas, asi que parece que la unica opcion es configurar un API gateway:

If you need the [`ecommerce-monorepo`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A59%2C%22character%22%3A17%7D%7D%5D%2C%225d9b012d-5509-46dd-8cd2-abc8985c928d%22%5D "Go to definition") container to be accessible through the internet while using a private IP address, you can set up an Azure Application Gateway or Azure Load Balancer to expose the container to the internet. This setup allows you to keep the container within the virtual network while still making it accessible externally.

### Steps to Implement

1. **Deploy [`ecommerce-monorepo`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A59%2C%22character%22%3A17%7D%7D%5D%2C%225d9b012d-5509-46dd-8cd2-abc8985c928d%22%5D "Go to definition") with a Private IP Address**: Deploy the container within the virtual network and subnet, using a private IP address.
2. **Set Up an Azure Application Gateway or Load Balancer**: Configure the Application Gateway or Load Balancer to route traffic from the internet to the [`ecommerce-monorepo`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A59%2C%22character%22%3A17%7D%7D%5D%2C%225d9b012d-5509-46dd-8cd2-abc8985c928d%22%5D "Go to definition") container.

### Updated Workflow

Here is the updated GitHub Actions workflow to deploy both containers with private IP addresses and set up an Azure Application Gateway to expose the [`ecommerce-monorepo`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A59%2C%22character%22%3A17%7D%7D%5D%2C%225d9b012d-5509-46dd-8cd2-abc8985c928d%22%5D "Go to definition") container:

```yaml
name: Deploy to ACI

on:
  push:
    branches:
      - unstable

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Log in to Docker Hub
      run: echo "${{ secrets.DOCKER_HUB_PAT }}" | docker login -u "${{ secrets.DOCKER_HUB_USERNAME }}" --password-stdin

    - name: Set up Azure CLI
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}

    - name: Ensure virtual network and subnet exist
      run: |
        if ! az network vnet show --resource-group laserantsgroup --name laserantsvnet; then
          az network vnet create --resource-group laserantsgroup --name laserantsvnet --address-prefix 10.0.0.0/16 --subnet-name lasersubnet --subnet-prefix 10.0.0.0/24
        fi

    - name: Ensure network security group exists
      run: |
        if ! az network nsg show --resource-group laserantsgroup --name laserantsnsg; then
          az network nsg create --resource-group laserantsgroup --name laserantsnsg --location westus
        fi

    - name: Ensure NSG rules allow internal communication
      run: |
        az network nsg rule create --resource-group laserantsgroup --nsg-name laserantsnsg --name AllowInternal --priority 100 --direction Inbound --access Allow --protocol Tcp --source-address-prefixes 10.0.0.0/16 --source-port-ranges '*' --destination-address-prefixes 10.0.0.0/16 --destination-port-ranges '*'

    - name: Associate NSG with subnet
      run: |
        az network vnet subnet update --resource-group laserantsgroup --vnet-name laserantsvnet --name lasersubnet --network-security-group laserantsnsg

    - name: Delete existing ecommerce-monorepo container group
      run: |
        if az container show --resource-group laserantsgroup --name ecommerce-monorepo; then
          az container delete --resource-group laserantsgroup --name ecommerce-monorepo --yes
        fi

    - name: Delete existing pgecommerce container group
      run: |
        if az container show --resource-group laserantsgroup --name pgecommerce; then
          az container delete --resource-group laserantsgroup --name pgecommerce --yes
        fi

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
          --environment-variables \
            DATABASE_URL=${{ secrets.DATABASE_URL }}

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
          --environment-variables \
            POSTGRES_DB=${{ secrets.POSTGRES_DB }} \
            POSTGRES_USER=${{ secrets.POSTGRES_USER }} \
            POSTGRES_PASSWORD=${{ secrets.POSTGRES_PASSWORD }}

    - name: Create public IP for Application Gateway
      run: |
        az network public-ip create --resource-group laserantsgroup --name appGatewayPublicIp --sku Standard --allocation-method Static

    - name: Create Application Gateway
      run: |
        az network application-gateway create \
          --resource-group laserantsgroup \
          --name appGateway \
          --location westus \
          --sku Standard_v2 \
          --capacity 2 \
          --vnet-name laserantsvnet \
          --subnet lasersubnet \
          --public-ip-address appGatewayPublicIp \
          --frontend-port 80 \
          --http-settings-port 5173 \
          --http-settings-protocol Http \
          --routing-rule-type Basic \
          --servers $(az container show --resource-group laserantsgroup --name ecommerce-monorepo --query ipAddress.ip --output tsv)
```

### Explanation

1. **Deploy Containers with Private IP Addresses**:
    ```yaml
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
          --environment-variables \
            DATABASE_URL=${{ secrets.DATABASE_URL }}

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
          --environment-variables \
            POSTGRES_DB=${{ secrets.POSTGRES_DB }} \
            POSTGRES_USER=${{ secrets.POSTGRES_USER }} \
            POSTGRES_PASSWORD=${{ secrets.POSTGRES_PASSWORD }}
    ```

2. **Create Public IP for Application Gateway**:
    ```yaml
    - name: Create public IP for Application Gateway
      run: |
        az network public-ip create --resource-group laserantsgroup --name appGatewayPublicIp --sku Standard --allocation-method Static
    ```

3. **Create Application Gateway**:
    ```yaml
    - name: Create Application Gateway
      run: |
        az network application-gateway create \
          --resource-group laserantsgroup \
          --name appGateway \
          --location westus \
          --sku Standard_v2 \
          --capacity 2 \
          --vnet-name laserantsvnet \
          --subnet lasersubnet \
          --public-ip-address appGatewayPublicIp \
          --frontend-port 80 \
          --http-settings-port 5173 \
          --http-settings-protocol Http \
          --routing-rule-type Basic \
          --servers $(az container show --resource-group laserantsgroup --name ecommerce-monorepo --query ipAddress.ip --output tsv)
    ```

### Summary

- **Deploy Containers with Private IP Addresses**: Both [`ecommerce-monorepo`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A59%2C%22character%22%3A17%7D%7D%5D%2C%225d9b012d-5509-46dd-8cd2-abc8985c928d%22%5D "Go to definition") and [`pgecommerce`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A51%2C%22character%22%3A68%7D%7D%5D%2C%225d9b012d-5509-46dd-8cd2-abc8985c928d%22%5D "Go to definition") containers are deployed within the virtual network using private IP addresses.
- **Set Up Application Gateway**: An Azure Application Gateway is created to expose the [`ecommerce-monorepo`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A59%2C%22character%22%3A17%7D%7D%5D%2C%225d9b012d-5509-46dd-8cd2-abc8985c928d%22%5D "Go to definition") container to the internet, routing traffic from a public IP address to the container's private IP address.

By following this updated workflow, you can ensure that the [`ecommerce-monorepo`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A59%2C%22character%22%3A17%7D%7D%5D%2C%225d9b012d-5509-46dd-8cd2-abc8985c928d%22%5D "Go to definition") container is accessible through the internet while still being deployed within the virtual network.


To access the [`ecommerce-monorepo`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A59%2C%22character%22%3A17%7D%7D%5D%2C%2216e62c19-6959-44c4-9b94-ec4553bfba2b%22%5D "Go to definition") container through the internet, you need to know the public IP address assigned to it. Since you are deploying the container with a public IP address, you can retrieve this information using the Azure CLI.

### Retrieve Public IP Address

You can use the following Azure CLI command to get the public IP address of the [`ecommerce-monorepo`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A59%2C%22character%22%3A17%7D%7D%5D%2C%2216e62c19-6959-44c4-9b94-ec4553bfba2b%22%5D "Go to definition") container:

```sh
az container show --resource-group laserantsgroup --name ecommerce-monorepo --query ipAddress.ip --output tsv
```

### Example Workflow Step to Retrieve and Output the Public IP Address

You can add a step to your GitHub Actions workflow to retrieve and output the public IP address of the [`ecommerce-monorepo`](command:_github.copilot.openSymbolFromReferences?%5B%22%22%2C%5B%7B%22uri%22%3A%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2F.github%2Fworkflows%2Fdeploy-to-aci.yml%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%22pos%22%3A%7B%22line%22%3A59%2C%22character%22%3A17%7D%7D%5D%2C%2216e62c19-6959-44c4-9b94-ec4553bfba2b%22%5D "Go to definition") container after it has been deployed.

### Updated Workflow

Here is the updated GitHub Actions workflow with an additional step to retrieve and output the public IP address:

```yaml

name: Deploy to ACI

  
on:

push:

branches:

- unstable

  

jobs:

deploy:

runs-on: ubuntu-latest

  

steps:

- name: Checkout code

uses: actions/checkout@v2

  

- name: Log in to Docker Hub

run: echo "${{ secrets.DOCKER_HUB_PAT }}" | docker login -u "${{ secrets.DOCKER_HUB_USERNAME }}" --password-stdin

  

- name: Set up Azure CLI

uses: azure/login@v1

with:

creds: ${{ secrets.AZURE_CREDENTIALS }}

  

- name: Ensure virtual network and subnets exist

run: |

if ! az network vnet show --resource-group laserantsgroup --name laserantsvnet; then

az network vnet create --resource-group laserantsgroup --name laserantsvnet --address-prefix 10.0.0.0/16 --subnet-name lasersubnet --subnet-prefix 10.0.1.0/24

fi

if ! az network vnet subnet show --resource-group laserantsgroup --vnet-name laserantsvnet --name appGatewaySubnet; then

az network vnet subnet create --resource-group laserantsgroup --vnet-name laserantsvnet --name appGatewaySubnet --address-prefix 10.0.2.0/24

fi

  

- name: Ensure network security group exists

run: |

if ! az network nsg show --resource-group laserantsgroup --name laserantsnsg; then

az network nsg create --resource-group laserantsgroup --name laserantsnsg --location westus

fi

  

- name: Ensure NSG rules allow internal communication

run: |

az network nsg rule create --resource-group laserantsgroup --nsg-name laserantsnsg --name AllowInternal --priority 100 --direction Inbound --access Allow --protocol Tcp --source-address-prefixes 10.0.0.0/16 --source-port-ranges '*' --destination-address-prefixes 10.0.0.0/16 --destination-port-ranges '*'

  

- name: Ensure NSG rules allow Application Gateway traffic

run: |

az network nsg rule create --resource-group laserantsgroup --nsg-name laserantsnsg --name AllowAppGateway --priority 200 --direction Inbound --access Allow --protocol Tcp --source-address-prefixes Internet --source-port-ranges '*' --destination-address-prefixes '*' --destination-port-ranges 65200-65535

  

- name: Associate NSG with subnet

run: |

az network vnet subnet update --resource-group laserantsgroup --vnet-name laserantsvnet --name lasersubnet --network-security-group laserantsnsg

  

- name: Delete existing ecommerce-monorepo container group

run: |

if az container show --resource-group laserantsgroup --name ecommerce-monorepo; then

az container delete --resource-group laserantsgroup --name ecommerce-monorepo --yes

fi

  

- name: Delete existing pgecommerce container group

run: |

if az container show --resource-group laserantsgroup --name pgecommerce; then

az container delete --resource-group laserantsgroup --name pgecommerce --yes

fi

  

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

--environment-variables \

DATABASE_URL=${{ secrets.DATABASE_URL }}

  

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

--environment-variables \

POSTGRES_DB=${{ secrets.POSTGRES_DB }} \

POSTGRES_USER=${{ secrets.POSTGRES_USER }} \

POSTGRES_PASSWORD=${{ secrets.POSTGRES_PASSWORD }}

  

- name: Create public IP for Application Gateway

run: |

az network public-ip create --resource-group laserantsgroup --name appGatewayPublicIp --sku Standard --allocation-method Static

  

- name: Create Application Gateway

run: |

az network application-gateway create \

--resource-group laserantsgroup \

--name appGateway \

--location westus \

--sku Standard_v2 \

--capacity 2 \

--vnet-name laserantsvnet \

--subnet appGatewaySubnet \

--public-ip-address appGatewayPublicIp \

--frontend-port 80 \

--http-settings-port 5173 \

--http-settings-protocol Http \

--routing-rule-type Basic \

--priority 1 \

--servers $(az container show --resource-group laserantsgroup --name ecommerce-monorepo --query ipAddress.ip --output tsv)

  

- name: Get public IP address of Application Gateway

run: |

APP_GATEWAY_IP=$(az network public-ip show --resource-group laserantsgroup --name appGatewayPublicIp --query ipAddress --output tsv)

echo "ecommerce-monorepo is accessible at http://$APP_GATEWAY_IP"

```

### Summary

- **Retrieve Public IP Address**: The `az container show` command is used to retrieve the public IP address of the [`ecommerce-monorepo`](vscode-file://vscode-app/usr/share/code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html) container.
- **Output Public IP Address**: The public IP address is outputted to the console, providing the URI to access the [`ecommerce-monorepo`](vscode-file://vscode-app/usr/share/code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html) container through the internet.

By following this updated workflow, you can easily retrieve and output the public IP address of the [`ecommerce-monorepo`](vscode-file://vscode-app/usr/share/code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html) container, allowing you to know the URI to access it through the internet.

==mucho ojo, esto fue al noveno intento, en esencia es un ejercicio de prueba y error:

![[Pasted image 20241001131637.png]]

![[Pasted image 20241001131813.png]]

## DEBUGGING 

![[Pasted image 20241001132335.png]]

==comandos para diagnosticar:

```bash

- az container list --resource-group laserantsgroup --output table

- az container logs --resource-group laserantsgroup --name pgecommerce
- az container logs --resource-group laserantsgroup --name ecommerce-monorepo

Verificar la IP de cada uno:
- az container show --resource-group laserantsgroup --name pgecommerce --query ipAddress.ip --output tsv
- az container show --resource-group laserantsgroup --name ecommerce-monorepo --query ipAddress.ip --output tsv

Entrar al container pgecommerce:
- az container exec --resource-group laserantsgroup --name pgecommerce --exec-command "/bin/bash"

Verificar si existe la base de datos y el usuario que el script crea:


- SELECT datname FROM pg_database WHERE datname = 'ecomdev';
- SELECT rolname FROM pg_roles WHERE rolname = 'laserants';

```

y aca me doy cuenta que a los containers no les estoy pasando los parametros que estan dentro del .env ==este es un tema bien importante porque es algo que uno hace en automatico en bare metal.

## Usando Azure Key Vault para gestionar secrets

Sure, let's go through the steps to use Azure Key Vault to manage your environment variables.

### Steps to Use Azure Key Vault

1. **Create an Azure Key Vault**: If you don't already have a Key Vault, you'll need to create one.
2. **Add Secrets to the Key Vault**: Store your environment variables as secrets in the Key Vault.
3. **Set Up Access Policies**: Ensure that your Azure service principal has access to the Key Vault.
4. **Retrieve Secrets in GitHub Actions**: Use the Azure Key Vault secrets in your GitHub Actions workflow.

### Step 1: Create an Azure Key Vault

Use the Azure CLI to create a Key Vault:

```sh
az keyvault create --name <your-keyvault-name> --resource-group <your-resource-group> --location <your-location>

az keyvault create --name laserantskeyvault --resource-group laserantsgroup --location westus

```


![[Pasted image 20241001135902.png]]
### Step 2: Add Secrets to the Key Vault

Add each environment variable as a secret in the Key Vault:

```sh
az keyvault secret set --vault-name <your-keyvault-name> --name BACKEND_PORT --value "8012"
az keyvault secret set --vault-name <your-keyvault-name> --name POSTGRES_USER --value "laserants"
az keyvault secret set --vault-name <your-keyvault-name> --name POSTGRES_PASSWORD --value "laserants"
az keyvault secret set --vault-name <your-keyvault-name> --name POSTGRES_DB --value "ecomdev"
az keyvault secret set --vault-name <your-keyvault-name> --name POSTGRES_HOST --value "pgecommerce"
az keyvault secret set --vault-name <your-keyvault-name> --name POSTGRES_PORT --value "5432"
az keyvault secret set --vault-name <your-keyvault-name> --name EXEC_MODE --value "development"
az keyvault secret set --vault-name <your-keyvault-name> --name SEED_DEVELOPMENT --value "true"
az keyvault secret set --vault-name <your-keyvault-name> --name SEED_PRODUCTION --value "true"
az keyvault secret set --vault-name <your-keyvault-name> --name FRONTEND_ORIGINS --value "http://localhost:5173"
az keyvault secret set --vault-name <your-keyvault-name> --name POSTGRES_DB_DEV_URL --value "postgresql://laserants:laserants@pgecommerce:5432/ecomdev"
az keyvault secret set --vault-name <your-keyvault-name> --name DATABASE_URL --value "postgresql://laserants:laserants@pgecommerce:5432/ecomdev"
az keyvault secret set --vault-name <your-keyvault-name> --name VITE_BACKEND_URL --value "http://localhost:8012"
az keyvault secret set --vault-name <your-keyvault-name> --name VITE_APP_NAME --value "MORGAN THE PIRATE: BUY AND SELL BAY"
```

==mucho ojo que las reglas de Azure Vault requiere que las var sean en minusculas y separados por normal score, por tanto me tocará hacer siempre un cambio sensible en el yaml del Action, tambien mi service principal necesita tener el rol de contribuidor

The error message indicates that the caller (your service principal) does not have the necessary permissions to set secrets in the Azure Key Vault. This is typically due to missing role assignments.

### Steps to Fix the Permission Issue

1. **Assign the Key Vault Contributor Role**: Assign the `Key Vault Contributor` role to your service principal for the Key Vault.
2. **Verify Role Assignment**: Ensure that the role assignment has propagated.

### Step 1: Assign the Key Vault Contributor Role

Use the Azure CLI to assign the `Key Vault Contributor` role to your service principal. Replace `<your-keyvault-name>`, `<your-subscription-id>`, and `<your-service-principal-id>` with appropriate values.

```sh
az role assignment create --role "Key Vault Contributor" --assignee <your-service-principal-id> --scope /subscriptions/<your-subscription-id>/resourceGroups/laserantsgroup/providers/Microsoft.KeyVault/vaults/<your-keyvault-name>


```

Para saber el ID del Service Principal:

```
az ad sp list --display-name "laserants-shoppingcart-app" --output table

el ID es el de la columna denominada AppId

SP_ID='1234zxxx'
```

### Example

```sh
az role assignment create --role "Key Vault Contributor" --assignee $SP_ID --scope /subscriptions/$SUBSCRIPTION_ID/resourceGroups/laserantsgroup/providers/Microsoft.KeyVault/vaults/laserantskeyvaul
```


![[Pasted image 20241001143216.png]]

### Step 2: Verify Role Assignment

Role assignments can take some time to propagate. You can verify the role assignment using the Azure CLI:

```sh
az role assignment list --assignee <your-service-principal-id> --scope /subscriptions/<your-subscription-id>/resourceGroups/laserantsgroup/providers/Microsoft.KeyVault/vaults/<your-keyvault-name>
```

### Example

```sh
az role assignment list --assignee $SP_ID --scope /subscriptions/$SUBSCRIPTION_ID/resourceGroups/laserantsgroup/providers/Microsoft.KeyVault/vaults/laserantskeyvaul
```


![[Pasted image 20241001143457.png]]

This command will list the role assignments for the specified service principal and scope. Ensure that the `Key Vault Contributor` role is listed.

==por favor esperar aca unos minutos para que los roles se propaguen, pero si aun asi no furual debo borrar y volver a crear el role:

```
az role assignment delete --assignee $SP_ID --role "Key Vault Contributor" --scope /subscriptions/$SUBSCRIPTION_ID/resourceG
roups/laserantsgroup/providers/Microsoft.KeyVault/vaults/laserantskeyvaul

az role assignment create --assignee $SP_ID --role "Key Vault Contributor" --scope /subscriptions/$SUBSCRIPTION_ID/resourceG
roups/laserantsgroup/providers/Microsoft.KeyVault/vaults/laserantskeyvaul

az keyvault set-policy --name laserantskeyvaul --spn $SP_ID --secret-permissions get list set delete

```


Si sigue dando problemas debo agregar estos permisos:
```
az role assignment create --assignee $SP_ID --role "Key Vault Secrets Officer" --scope /subscriptions/$SUBSCRIPTION_ID/resourceGroups/laserantsgroup/providers/Microsoft.KeyVault/vaults/laserantskeyvaul

az role assignment list --assignee $SP_ID --scope /subscriptions/$SUBSCRIPTION_ID/resourceGroups/laserantsgroup/providers/Microsoft.KeyVault/vaults/laserantskeyvaul


```

### Summary

- **Assign the Key Vault Contributor Role**: Use the Azure CLI to assign the `Key Vault Contributor` role to your service principal.
- **Verify Role Assignment**: Ensure that the role assignment has propagated and is listed.

By following these steps, you can resolve the permission issue and set secrets in the Azure Key Vault. Let me know if you encounter any other issues or if you're ready to proceed with the next steps.

==tratando de crear los secrets:

```bash
az keyvault secret set --vault-name laserantskeyvaul --name backendport --value "8012"
az keyvault secret set --vault-name laserantskeyvaul --name postgresuser --value "laserants"
az keyvault secret set --vault-name laserantskeyvaul --name postgrespassword --value "laserants"
az keyvault secret set --vault-name laserantskeyvaul --name postgresdb --value "ecomdev"
az keyvault secret set --vault-name laserantskeyvaul --name postgreshost --value "pgecommerce"
az keyvault secret set --vault-name laserantskeyvaul --name postgresport --value "5432"
az keyvault secret set --vault-name laserantskeyvaul --name execmode --value "development"
az keyvault secret set --vault-name laserantskeyvaul --name seeddevelopment --value "true"
az keyvault secret set --vault-name laserantskeyvaul --name seedproduction --value "true"
az keyvault secret set --vault-name laserantskeyvaul --name frontendorigins --value "http://localhost:5173"
az keyvault secret set --vault-name laserantskeyvaul --name postgresdbdevurl --value "postgresql://laserants:laserants@pgecommerce:5432/ecomdev"
az keyvault secret set --vault-name laserantskeyvaul --name databaseurl --value "postgresql://laserants:laserants@pgecommerce:5432/ecomdev"
az keyvault secret set --vault-name laserantskeyvaul --name vitebackendurl --value "http://localhost:8012"
az keyvault secret set --vault-name laserantskeyvaul --name viteappname --value "MORGAN THE PIRATE: BUY AND SELL BAY"

```


para verificar si el seteo de cada secret fue exitoso tiene que ser una salida como esta:

![[Pasted image 20241002113642.png]]

### Step 3: Set Up Access Policies

Ensure that your Azure service principal has access to the Key Vault. You can set up an access policy using the Azure CLI:

```sh
az keyvault set-policy --name <your-keyvault-name> --spn <your-service-principal-id> --secret-permissions get
```

Replace `<your-keyvault-name>` with the name of your Key Vault and `<your-service-principal-id>` with the ID of your service principal.

### Step 4: Retrieve Secrets in GitHub Actions

You will need to use the `azure/keyvault-secrets` GitHub Action to retrieve the secrets from the Key Vault. This will be shown in the updated YAML file when you ask for it.

By following these steps, you can securely store and manage your environment variables using Azure Key Vault. Let me know when you're ready to see the updated YAML file with these changes.

## actualizacion en el github actions:

```yaml
name: Deploy to ACI

on:
  push:
    branches:
      - unstable

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Log in to Docker Hub
      run: echo "${{ secrets.DOCKER_HUB_PAT }}" | docker login -u "${{ secrets.DOCKER_HUB_USERNAME }}" --password-stdin

    - name: Set up Azure CLI
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}

    - name: Retrieve secrets from Azure Key Vault
      id: keyvault
      uses: azure/keyvault-secrets@v1
      with:
        keyvault: laserantskeyvaul
        secrets: |
          backendport
          postgresuser
          postgrespassword
          postgresdb
          postgreshost
          postgresport
          execmode
          seeddevelopment
          seedproduction
          frontendorigins
          postgresdbdevurl
          databaseurl
          vitebackendurl
          viteappname

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
          --environment-variables \
            BACKEND_PORT=${{ steps.keyvault.outputs.backendport }} \
            POSTGRES_USER=${{ steps.keyvault.outputs.postgresuser }} \
            POSTGRES_PASSWORD=${{ steps.keyvault.outputs.postgrespassword }} \
            POSTGRES_DB=${{ steps.keyvault.outputs.postgresdb }} \
            POSTGRES_HOST=${{ steps.keyvault.outputs.postgreshost }} \
            POSTGRES_PORT=${{ steps.keyvault.outputs.postgresport }} \
            EXEC_MODE=${{ steps.keyvault.outputs.execmode }} \
            SEED_DEVELOPMENT=${{ steps.keyvault.outputs.seeddevelopment }} \
            SEED_PRODUCTION=${{ steps.keyvault.outputs.seedproduction }} \
            FRONTEND_ORIGINS=${{ steps.keyvault.outputs.frontendorigins }} \
            POSTGRES_DB_DEV_URL=${{ steps.keyvault.outputs.postgresdbdevurl }} \
            DATABASE_URL=${{ steps.keyvault.outputs.databaseurl }} \
            VITE_BACKEND_URL=${{ steps.keyvault.outputs.vitebackendurl }} \
            VITE_APP_NAME=${{ steps.keyvault.outputs.viteappname }}

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
          --environment-variables \
            POSTGRES_USER=${{ steps.keyvault.outputs.postgresuser }} \
            POSTGRES_PASSWORD=${{ steps.keyvault.outputs.postgrespassword }} \
            POSTGRES_DB=${{ steps.keyvault.outputs.postgresdb }} \
            POSTGRES_HOST=${{ steps.keyvault.outputs.postgreshost }} \
            POSTGRES_PORT=${{ steps.keyvault.outputs.postgresport }} \
            EXEC_MODE=${{ steps.keyvault.outputs.execmode }} \
            DATABASE_URL=${{ steps.keyvault.outputs.databaseurl }}

    - name: Create public IP for Application Gateway
      run: |
        az network public-ip create --resource-group laserantsgroup --name appGatewayPublicIp --sku Standard --allocation-method Static

    - name: Create Application Gateway
      run: |
        az network application-gateway create \
          --resource-group laserantsgroup \
          --name appGateway \
          --location westus \
          --sku Standard_v2 \
          --capacity 2 \
          --vnet-name laserantsvnet \
          --subnet appGatewaySubnet \
          --public-ip-address appGatewayPublicIp \
          --frontend-port 80 \
          --http-settings-port 5173 \
          --http-settings-protocol Http \
          --routing-rule-type Basic \
          --priority 1 \
          --servers $(az container show --resource-group laserantsgroup --name ecommerce-monorepo --query ipAddress.ip --output tsv)

    - name: Create custom health probe
      run: |
        az network application-gateway probe create \
          --resource-group laserantsgroup \
          --gateway-name appGateway \
          --name customHealthProbe \
          --protocol Http \
          --host-name-from-http-settings true \
          --path / \
          --interval 30 \
          --timeout 30 \
          --unhealthy-threshold 3

    - name: Update HTTP settings with custom health probe
      run: |
        az network application-gateway http-settings update \
          --resource-group laserantsgroup \
          --gateway-name appGateway \
          --name appGatewayBackendHttpSettings \
          --probe customHealthProbe

    - name: Get public IP address of Application Gateway
      run: |
        APP_GATEWAY_IP=$(az network public-ip show --resource-group laserantsgroup --name appGatewayPublicIp --query ipAddress --output tsv)
        echo "ecommerce-monorepo is accessible at http://$APP_GATEWAY_IP"
```

