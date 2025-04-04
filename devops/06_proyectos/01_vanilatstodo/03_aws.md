## IAM

- usar el enfoque de least privilege
- norma: mi usuario root jamás se debe utilizar fuera de la consola aws, lease: no ser utilizado para configurar addons o corre pipelines, entre otros
- los usuarios IAM sólo deben tener los privilegtios que requieren
- el siguiente ejemplo es de muy mala practica:

![[Pasted image 20250327123801.png]]

- puesto que es una lata estar buscando privilegio por privilegio, lo mejor es hacer un script para creacion de cuentas y aplicacion de permisos

### Roles IAM en mi ecosistema:

1. Administrator: gestionar perfiles IAM
2. idedev: utilizar este perfil en los IDE para desarrollo y que tienen extensiones para AWS, asi como AWS Toolkit
3. 

### Script para eliminar un usuario y sus keys:

```
#hacer un listado de las policies asignadas
aws iam list-attached-user-policies --user-name devopsadmin

# hay que borrar policy por policy, sale más facil ir al entorno grafico y hacerlo desde ahi

#alternativamente este script puede funcionar pero lo mejor es refrescar la pantalla:
aws iam list-attached-user-policies --user-name devopsadmin | \
jq -r '.AttachedPolicies[].PolicyArn' | \
while read -r policy_arn; do
echo "Detaching policy: $policy_arn"
aws iam detach-user-policy --user-name devopsadmin --policy-arn "$policy_arn"
done

#delete the login profile:
aws iam delete-login-profile --user-name devopsadmin

# Delete access keys
aws iam list-access-keys --user-name devopsadmin | \
jq -r '.AccessKeyMetadata[].AccessKeyId' | \
while read -r key_id; do
    aws iam delete-access-key --user-name devopsadmin --access-key-id "$key_id"
done

# Finally delete the user
aws iam delete-user --user-name devopsadmin

#idealmente este script debe ser ejecutado desde aws-cli, en CloudShell despues de cada comando hay que refrescar

#verifying
aws iam get-user --user-name devopsadmin || echo "User successfully deleted"

```

==probando este script para eliminar el usuario hftamayodevops desde cloud shell:

recordar que perfiles administrativos sólo deben ser usados en entornos controlados como es el caso de CloudShell:

![[Pasted image 20250327150122.png]]


```
aws iam list-attached-user-policies --user-name hftamayodevops

aws iam list-attached-user-policies --user-name hftamayodevops | \
jq -r '.AttachedPolicies[].PolicyArn' | \
while read -r policy_arn; do
echo "Detaching policy: $policy_arn"
aws iam detach-user-policy --user-name hftamayodevops --policy-arn "$policy_arn"
done

aws iam delete-login-profile --user-name hftamayodevops

aws iam list-access-keys --user-name hftamayodevops | \
jq -r '.AccessKeyMetadata[].AccessKeyId' | \
while read -r key_id; do
    aws iam delete-access-key --user-name hftamayodevops --access-key-id "$key_id"
done

#si el usuario tiene credenciales atachadas a un servicio:
aws iam list-service-specific-credentials --user-name hftamayodevops

# Finally delete the user
aws iam delete-user --user-name hftamayodevops

# como ultimo recurso borrarlo en el dashboard grafico

```


![[Pasted image 20250327150441.png]]

![[Pasted image 20250327150529.png]]

![[Pasted image 20250327151102.png]]

## Policies:
vanillatstodo_terraform: contiene los permisos iam necesarios para infra que terraform requiere para funcionar