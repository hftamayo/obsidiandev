
![[Pasted image 20240915164513.png]]

![[Pasted image 20240915165254.png]]

==aca me dio un error, entonces voy a cambiar el comando, sin crear el service principal el AKS no podrá ser creado:





![[Pasted image 20240915165410.png]]

==punto bien importante:

![[Pasted image 20240915165525.png]]

==todos los nombres deben ser lowercase

==generando el AKS cluster

![[Pasted image 20240915165753.png]]

```
SSH key files '/home/hftamayo/.ssh/id_rsa' and '/home/hftamayo/.ssh/id_rsa.pub' have been generated under ~/.ssh to allow SSH access to the VM. If using machines without permanent storage like Azure Cloud Shell without an attached file share, back up your keys to a safe location
Values of identifierUris property must use a verified domain of the organization or its subdomain: 'https://beaa9f.laserantsa-laserantsGroup-e8698c.westus.cloudapp.azure.com'
```

## ==ok, los pasos anteriores en buena medida fallaron por el uso del Azure Cli de Uubuntu, por qué a saber, aca la actualizacion del proceso:


```
az group create -n laserantsgroup -l westus

SUBSCRIPTION_ID=$(az account list --query "[?isDefault].id" -o tsv)
echo $SUBSCRIPTION_ID

az ad sp create-for-rbac --name laserants-ecommerce-app --role contributor --scopes /subscriptions/$SUBSCRIPTION_ID/resourceGroups/laserantsgroup --sdk-auth

az resource list --resource-group laserantsgroup

```

![[Pasted image 20240916093104.png]]

==creando el registry para los contenedores:

![[Pasted image 20240916093337.png]]

==como obtener las AZURE CREDENTIALS:

- esto es bien importante porque es la salida de datos cuando el sp fue creado ==y una vez que fueron mostradas ya no puede recuperarse pues son como secrets ==
- la opcion que tengo es generar otro secret y construir las credenciales manualmente o borrarlo y volverlo a crear:

![[Pasted image 20240916094234.png]]

==creando un nuevo sp y obteniendo la cred:
![[Pasted image 20240916094844.png]]

==listando el sp:

![[Pasted image 20240916100028.png]]

==asignado rol al sp para gestionar AKS:

```
az role assignment create --assignee $SP_APPID --scope /subscriptions/$SUBSCRIPTION_ID/resourceGroups/laserantsgroup/providers/Microsoft.ContainerRegistry/registries/laserantsregistry --role acrpull
```

![[Pasted image 20240916100840.png]]

==creando el AKS cluster: en esta paret voy a necesitar el AZURE_CREDENTIALS una parte que se llama CLIENT SECRET:

![[Pasted image 20240916101635.png]]

==aca dice que no estoy registrado para usar el servicio AKS, tratando de resolverlo:

![[Pasted image 20240916102335.png]]

![[Pasted image 20240916102422.png]]

==verificando quotas:

![[Pasted image 20240916102725.png]]

pues parece que no tengo quotas asignadas por region, verificando en el portal:

![[Pasted image 20240916103658.png]]

![[Pasted image 20240916103742.png]]

==pidiendo que me aumenten quotas:

![[Pasted image 20240916104409.png]]

![[Pasted image 20240916104511.png]]

![[Pasted image 20240916104629.png]]

![[Pasted image 20240916110102.png]]

==vaya entonces lo que hice fue acudir al servicio de AzureSupport en Twitter y puse la pergunta si puedo trabajar con AKS si o no, sin embargo en la foto que tome de la quota aparece este mensaje:

The selected provider is not registered for some of the selected subscriptions. To access your quotas y este link: https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/resource-providers-and-types



