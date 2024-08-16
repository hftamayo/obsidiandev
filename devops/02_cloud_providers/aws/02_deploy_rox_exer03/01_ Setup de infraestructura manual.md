
==la idea será montar una EC2 que me servirá de estacion de trabajo y desde ahi voy a controlar mi Infraestructura, esto con el objetivo de no cargar mi estacion de trabajo dev, pero parece que el equipo DevOps si debería tener estaciones de trabajo para estas operaciones

1. instalar paquetes indispensables en mi equipo:

```
sudo apt install awscli
```


# ETAPA 1: CREACION DE CREDENCIALES


2. crear una cuenta de menor privilegio para estas operaciones:

![[Pasted image 20240814153908.png]]

![[Pasted image 20240814153942.png]]

![[Pasted image 20240814154115.png]]

![[Pasted image 20240814154539.png]]

![[Pasted image 20240814154609.png]]

![[Pasted image 20240814154642.png]]

![[Pasted image 20240814154841.png]]

![[Pasted image 20240814154917.png]]

![[Pasted image 20240814154959.png]]

Si uso el botón Downad csv file descargo las creds del usuario en formato CSV,

Para obtener el Access Key ID y el Secret Access Key presiono en el boton View user y voy a esta pantalla:

![[Pasted image 20240814155433.png]]

![[Pasted image 20240814155512.png]]

![[Pasted image 20240814155557.png]]

![[Pasted image 20240814155619.png]]

![[Pasted image 20240814155648.png]]

![[Pasted image 20240814155748.png]]

en el archivo descargado tengo la info que requiero para configurar mi awscli

3. configurar mi access key en awscli:
![[Pasted image 20240815124250.png]]

==para la seleccion de la region:
![[Pasted image 20240815124418.png]]


4.  Habilitando MFA para el nuevo usuario (usando un mismo dispositivo)

![[Pasted image 20240815125131.png]]

![[Pasted image 20240815125150.png]]

![[Pasted image 20240815125439.png]]

![[Pasted image 20240815125454.png]]

![[Pasted image 20240815125711.png]]


# ETAPA 2: ASIGNACION DE UN IAM ROLE PARA OPS CON EC2 INSTANCES 

Puesto que es más seguro usar IAM Roles para ciertos servicios pues no se requiere guardar llaves en dichas instancias, se sugiere crear roles como el siguiente:

![[Pasted image 20240815130147.png]]

![[Pasted image 20240815130254.png]]

![[Pasted image 20240815130325.png]]

![[Pasted image 20240815130400.png]]

![[Pasted image 20240815130642.png]]

![[Pasted image 20240815130657.png]]

![[Pasted image 20240815130756.png]]

![[Pasted image 20240815130820.png]]

# ETAPA 3: CREACION DE MI INSTANCIA EC2 QUE SERVIRA DE TERMINAL

![[Pasted image 20240815131747.png]]

![[Pasted image 20240815131834.png]]

![[Pasted image 20240815131945.png]]

![[Pasted image 20240815132138.png]]

![[Pasted image 20240815132228.png]]

==la siguienet configuracion es muy laxa:

![[Pasted image 20240815132509.png]]

![[Pasted image 20240815132535.png]]

![[Pasted image 20240815132639.png]]

![[Pasted image 20240815132806.png]]

# ETAPA 4:  CREANDO UNA ALTERA DE BILLING

![[Pasted image 20240815141247.png]]

![[Pasted image 20240815141347.png]]

# ETAPA 5: ASIGNANDO EL ROL IAM A LA EC2

![[Pasted image 20240815142019.png]]

![[Pasted image 20240815142422.png]]


![[aws_modify_iam.gif]]


seleccion del role:

![[Pasted image 20240815144634.png]]

![[Pasted image 20240815145110.png]]

# ETAPA 6: PROBAR LA CONEXION A LA EC2

agregando los permisos al pem file:

![[Pasted image 20240816151045.png]]

==aca me dio error porque la cuenta default ec2-user es para amazon linux, para debian es admin


![[Pasted image 20240816151416.png]]

==verificando el IAM role y si puedo hacer uso de comandos aws-cli sin instalar keys:

![[Pasted image 20240816151707.png]]

# ETAPA 7: CONFIGURANDO LA VM CON LA PAQUETERIA NECESARIA

![[Pasted image 20240816152153.png]]

![[Pasted image 20240816152420.png]]

![[Pasted image 20240816153748.png]]

# ETAPA 8: 

# ETAPA 9:
