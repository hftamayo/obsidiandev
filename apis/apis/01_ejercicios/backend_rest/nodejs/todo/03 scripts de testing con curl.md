
Para probarlo en el OJ todo tiene que salir por proxychains:

levantando el tunel con el server 10.0.0.65:

![[Pasted image 20230626125606.png]]

probando si curl esta saliendo sin pasar por el proxy:

![[Pasted image 20230626125639.png]]

levantando la aplicacion por medio de proxychains:

![[Pasted image 20230626125715.png]]

1. POST Request: Add a New User

curl -X POST http://localhost:5000/api/users/register -H "Content-Type: application/json" -d '{"name": "Herbert Tamayo", "email":"hftamayo@gmail.com", "password":"123456", "age": "22"}'

![[Pasted image 20230626125826.png]]

* Actualizacion: favor referirse a nodetodo en lugar de api en todos los endpoints

![[Pasted image 20230711105639.png]]


3. POST Request: Login
curl -X POST http://localhost:5000/api/users/login -H "Content-Type: application/json" -d '{"email": "hftamayo@gmail.com", "password":"123456"}'


![[Pasted image 20230626130029.png]]

Obteniendo datos mas especificos:

curl -i -X POST http://localhost:5000/api/users/login -H "Content-Type: application/json" -d '{"email": "sebas@gmail.com", "password":"123456"}'


![[Pasted image 20230702171601.png]]

notese que con -i puedo ver la cookie que se genera en el handshake:

token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjoiNjRhMjAwNWIzZTAwYjM4ZTZhNDJmZTlhIiwiaWF0IjoxNjg4MzQxMDE3LCJleHAiOjE2ODg3MDEwMTd9.QHsC3iAc2ylx08NJblJSyaxoFAgjtlw13tE1910uhVo


2.  GET Request: Logout
curl http://localhost:5000/api/users/logout

![[Pasted image 20230626130145.png]]

Obteniendo mayores datos:
curl -i -X GET http://localhost:5000/api/users/logout

![[Pasted image 20230702171358.png]]


En los anteriores request aparece que no estoy autorizado debido a que necesito pasar la cookie de autorizacion que generé en el login, para resolver esto sigo estos pasos:

a.  en el proceso de login y mediante el modificador -i obtuve la cookie generada, esa la uso en los endpoints, por ejemplo en logout:

![[Pasted image 20230702173931.png]]

b. para no tener que escribir toda la cookie mejor la copio en un arcivo


![[Pasted image 20230702173539.png]]


c. ahora llamando el archivo que contiene la cookie en un request el archivo anterior no funciona pues se requiere de un formato especifico.

d. generando un archivo contenedor de cookies desde el metodo POST login:

curl --cookie-jar sessions.txt -i -X POST http://localhost:5000/api/users/login -H "Content-Type: application/json" -d '{"email": "sebas@gmail.com", "password":"123456"}'

![[Pasted image 20230702175111.png]]

probando hacer login con el nombre de endpoints distinto y con CORS activado:

![[Pasted image 20230711105948.png]]


e. haciendo uso del archivo contenedor para un request:

![[Pasted image 20230702175142.png]]


4. GET Request: Active Session
curl http://localhost:5000/api/users/me

![[Pasted image 20230626130142.png]]

![[Pasted image 20230702175337.png]]

Usando el endpoint me con CORS activado:

![[Pasted image 20230711110556.png]]


Esta misma situacion aplica para update y delete record

5. PUT Request: update record
curl -X PUT http://localhost:5000/api/users/updatedetails -H "Content-Type: application/json" -d  '{"name": "herbert tamayo", "email": "hftamayo@gmail.com", "age": "35"}'

![[Pasted image 20230702175628.png]]

Probando el mismo endpoint con el CORS activado:

![[Pasted image 20230711110923.png]]

![[Pasted image 20230711110956.png]]


6. PUT Request: update password
curl -X PUT http://localhost:5000/api/users/updatepassword -H "Content-Type: application/json" -d '{"password": "123456", "newPassword": "123456chivo"}'

![[Pasted image 20230702175919.png]]

probando el mismo endpoint con CORS:

![[Pasted image 20230711111246.png]]

probando la nueva clave:

![[Pasted image 20230702180236.png]]

con cors:

![[Pasted image 20230711111344.png]]

probando con las credenciales nuevas:

![[Pasted image 20230702180311.png]]

![[Pasted image 20230702180506.png]]

haciendo lo mismo con CORS:

![[Pasted image 20230711111541.png]]

![[Pasted image 20230711111614.png]]


7. DELETE Request: delete a user
curl -X DELETE http://localhost:5000/api/users/delete

![[Pasted image 20230702180552.png]]

por el siguiente error es que no puedo ejecutar este endpoint:

![[Pasted image 20230702190249.png]]

En la actualizacion de Mongoose disponible en 2023 remove fue sustituido por deleteOne, sin embargo al hacer el cambio en el codigo obtengo el mismo error.

tratando de obtener mayores detalles:

![[Pasted image 20230702191355.png]]

en los anteriores request me equivoque pues no estaba poniendo el -X DELETE, le actualicé el nombre del endpoint, corrigiendo y probando:

![[Pasted image 20230702192335.png]]

probando este endpoint con CORS:

![[Pasted image 20230711111744.png]]


![[Pasted image 20230702192418.png]]

el token que se ve en el archivo es de otra sesion de usuario.

TODOS:

Iniciando sesion:

![[Pasted image 20230702193544.png]]


8. POST REQUEST: create a task
curl -X POST http://localhost:5000/api/todos/create -H "Content-Type: application/json" -d '{"title": "go to the supermarket", "description": "cheese, fruits, veggies"}'

![[Pasted image 20230702193635.png]]

9. GET REQUEST: display all tasks
curl http://localhost:5000/api/todos/

![[Pasted image 20230702193718.png]]


10. GET REQUEST: display info of a task in particular
curl http://localhost:5000/api/todos/63xcff...

![[Pasted image 20230702194532.png]]

11. PUT REQUEST: update task information
curl -X PUT http://localhost:5000/api/todos/update/63xcff... -H "Content-Type: application/json" -d '{"title": "my updated title", "description": "my updated description", "completed": "true"}'

12. DELETE REQUEST: delete a task
curl -X DELETE http://localhost:5000/api/todos/delete/63xcff... 

![[Pasted image 20230702194725.png]]


