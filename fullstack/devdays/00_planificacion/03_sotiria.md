PROJECT'S INSTALL AND SETUP
=

![[Pasted image 20240425103508.png]]

![[Pasted image 20240425103538.png]]

![[Pasted image 20240425103627.png]]


![[Pasted image 20240425103835.png]]

![[Pasted image 20240425103901.png]]

==instalando y configurando Tailwind

![[Pasted image 20240426075634.png]]

![[Pasted image 20240426075755.png]]

![[Pasted image 20240426082920.png]]

![[Pasted image 20240426080036.png]]

![[Pasted image 20240426080133.png]]

![[Pasted image 20240426080209.png]]

==script de prueba para verificar funcionamiento de Tailwind

![[Pasted image 20240426083009.png]]

==traduciendo postcss y tailwind para que el codigo sea compatible con ES module syntax:

![[Pasted image 20240426083044.png]]

![[Pasted image 20240426083056.png]]

==la version 3.x de Tailwind requiere un cambio en el uso del pre-processor jit, modificando el codigo de mis config files

![[Pasted image 20240426083453.png]]

![[Pasted image 20240426083508.png]]

==luego de eso elimino node_modules, hago un pnpm install y luego un pnpm run dev

![[Pasted image 20240426083630.png]]


![[Pasted image 20240429080051.png]]

==CONTENEDOR DE LA APLICACION
=

- Este es el Dockerfile:

![[Pasted image 20240517075822.png]]


- Estado del Container:

![[Pasted image 20240517075905.png]]

![[Pasted image 20240517075949.png]]

![[Pasted image 20240517080020.png]]


- Sin embargo no conecta:

![[Pasted image 20240517080106.png]]

![[Pasted image 20240517080158.png]]

==y esto es porque Vite restringue conexiones que no vengan de Localhost, asi que en el vite.config se hace el siguiente cambio:

![[Pasted image 20240517082446.png]]

![[Pasted image 20240517083032.png]]

- ![[Pasted image 20240517083204.png]]


### ==DISEÑO DE LA ENTIDAD TASK:

NOMBRE DE LA TABLA: ROLES
- ID
- NAME
- DESCRIPTION
- STATUS
- PERMISSIONS
- TIMESTAMPS (CREATEDAT, UPDATEDAT, DELETEDAT)

NOMBRE DE LA TABLA: USERS
- ID
- LASTNAME
- FIRSTNAME
- BIRTH_DATE
- EMAIL
- PASSWORD
- STATUS
- ROLE
- TASKS


NOMBRE DE LA TABLA: TASK

- ID
- TITLE
- DESCRIPTION
- DONE
- OWNER
- TIMESTAMPS (CREATEDAT, UPDATEDAT, DELETEDAT)