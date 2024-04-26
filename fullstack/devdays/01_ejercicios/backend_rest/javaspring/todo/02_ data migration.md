
javaspringboot hace de manera automatica la migration de la base de datos, es incluye:
- seleccion de la base de datos establecida en un archivo properties
- creacion de las tablas y relaciones necesarias establecidas en los models
- el usuario y clave de conexion debe haber sido creado previamente por un YAML en el DBMS

Aca la secuencia de conexion desde springBoot hacia un MySQL en Docker, unicamente existe la DB y el usuario con los privilegios necesarios:

1. desde IntelliJ le di Run al script de ejecución (TodoApplication)
2. despues de interpretar obtuve esta salida:

![[Pasted image 20230816105543.png]]


3.  verificando la creación de las tablas:

![[Pasted image 20230816105802.png]]

![[Pasted image 20230816105833.png]]

![[Pasted image 20230816105904.png]]

automaticamente la estructura de la tabla se creo

4. en la version 1 TODO no tiene usuarios, categorias default o data que es necesaria para arrancar la aplicaicon (como un admin), para futuras versiones se incluirá migraciones