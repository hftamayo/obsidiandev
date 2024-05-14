
USER WORFLOW:
=

==REGISTER:==

curl -X POST http://10.2.0.238:8003/nodetodo/users/register -H "Content-Type: application/json" -d '{"name": "Wilfrido Vargas", "email":"wilfrido@merengue.com", "password":"password123", "age": "22"}'

==output:

{"httpStatusCode":200,"resultMessage":"User created successfully","newUser":{"name":"Wilfrido Vargas","email":"wilfrido@merengue.com","age":22,"_id":"65cf89c6cabbdb52f571209c","createdAt":"2024-02-16T16:13:58.056Z","updatedAt":"2024-02-16T16:13:58.056Z","__v":0}}


==LOGIN

curl --cookie-jar sessions.txt -i -X POST http://10.2.0.238:8003/nodetodo/users/login -H "Content-Type: application/json" -d '{"email": "wilfrido@merengue.com", "password":"password123"}'


==output:

{"httpStatusCode":200,"resultMessage":"User login successfully","loggedUser":{"_id":"65cf89c6cabbdb52f571209c","name":"Wilfrido Vargas","email":"wilfrido@merengue.com","age":22,"createdAt":"2024-02-16T16:13:58.056Z","updatedAt":"2024-02-08T16:10:08.461Z","__v":0}}

==GET DETAILS:

curl -i -b sessions.txt http://10.2.0.238:8003/nodetodo/users/me

curl -i -b sessions.txt http://localhost:8003/nodetodo/users/me

==output:

{"httpStatusCode":200,"resultMessage":"User Found","searchUser":{"_id":"65cf89c6cabbdb52f571209c","name":"Wilfrido Vargas","email":"wilfrido@merengue.com","age":22,"createdAt":"2024-02-16T16:13:58.056Z","updatedAt":"2024-02-16T16:13:58.056Z","__v":0}}

==UPDATE PASSWORD:

curl -i -b sessions.txt -X PUT http://10.2.0.238:8003/nodetodo/users/updatepassword -H "Content-Type: application/json" -d '{"password": "password123", "newPassword": "password1234"}'

==output:==
{"httpStatusCode":200,"resultMessage":"Password updated successfully","updatedUser":{"_id":"65cf89c6cabbdb52f571209c","name":"Wilfrido Vargas","email":"wilfrido@merengue.com","age":22,"createdAt":"2024-02-16T16:13:58.056Z","updatedAt":"2024-02-16T16:25:30.249Z","__v":0}}

==LOGOUT :==

curl -i -b sessions.txt -X POST http://10.2.0.238:8003/nodetodo/users/logout

==output:==

{"httpStatusCode":200,"resultMessage":"User logged out successfully"}

==probando la nueva clave:==

curl --cookie-jar sessions.txt -i -X POST http://10.2.0.238:8003/nodetodo/users/login -H "Content-Type: application/json" -d '{"email": "wilfrido@merengue.com", "password":"password1234"}'

{"httpStatusCode":200,"resultMessage":"User login successfully","loggedUser":{"_id":"65cf89c6cabbdb52f571209c","name":"Wilfrido Vargas","email":"wilfrido@merengue.com","age":22,"createdAt":"2024-02-16T16:13:58.056Z","updatedAt":"2024-02-16T16:25:30.249Z","__v":0}}

==UPDATE DETAILS:

curl  -i -b sessions.txt -X PUT http://10.2.0.238:8003/nodetodo/users/updatedetails -H "Content-Type: application/json" -d  '{"name": "herbert tamayo", "email": "hftamayo@gmail.com", "age": "35"}'

==output:==

{"httpStatusCode":200,"resultMessage":"Data updated successfully","updatedUser":{"_id":"65cf89c6cabbdb52f571209c","name":"herbert tamayo","email":"hftamayo@gmail.com","age":35,"createdAt":"2024-02-16T16:13:58.056Z","updatedAt":"2024-02-16T16:47:36.998Z","__v":0}}

==DELETE USER:==

curl -i -b sessions.txt -X DELETE http://10.2.0.238:8003/nodetodo/users/deleteuser

{"httpStatusCode":200,"resultMessage":"User deleted successfully"}

TODO WORKFLOW (REQUIRES ACTIVE SESSION):
=

==NEW:==
curl -i -b sessions.txt -X POST http://10.2.0.238:8003/nodetodo/todos/create -H "Content-Type: application/json" -d '{"title": "go to the supermarket", "description": "cheese, fruits, veggies"}'

==output:==

{"httpStatusCode":200,"resultMessage":"Todo created successfully","newTodo":{"title":"go to the supermarket","description":"cheese, fruits, veggies","completed":false,"user":"65cb80af449a37007de1e826","_id":"65cf94a7cabbdb52f57120ac","createdAt":"2024-02-16T17:00:23.068Z","updatedAt":"2024-02-16T17:00:23.068Z","__v":0}}

==GET ALL TODOS:==
curl -i -b sessions.txt -X GET http://10.2.0.238:8003/nodetodo/todos/list

==output:==

{"httpStatusCode":200,"resultMessage":"Tasks found","activeTodos":[{"_id":"65cf94a7cabbdb52f57120ac","title":"go to the supermarket","description":"cheese, fruits, veggies","completed":false,"user":"65cb80af449a37007de1e826","createdAt":"2024-02-16T17:00:23.068Z","updatedAt":"2024-02-16T17:00:23.068Z","__v":0}]}

==GET A TODO:==
curl -i -b sessions.txt -X GET http://10.2.0.238:8003/nodetodo/todos/task/65cf94a7cabbdb52f57120ac

==output:==

{"httpStatusCode":200,"resultMessage":"Todo found","searchTodo":{"_id":"65cf94a7cabbdb52f57120ac","title":"go to the supermarket","description":"cheese, fruits, veggies","completed":false,"user":"65cb80af449a37007de1e826","createdAt":"2024-02-16T17:00:23.068Z","updatedAt":"2024-02-16T17:00:23.068Z","__v":0}}


==UPDATE:

curl -i -b sessions.txt -X PUT http://10.2.0.238:8003/nodetodo/todos/update/5f7f8b1e9f3f9c1d6c1e4d1d -H "Content-Type: application/json" -d '{"title": "this is my new update", "description": "this is my new description", "completed": true}'

==output:==

{"resultMessage":"Todo updated successfully","updateTodo":{"_id":"5f7f8b1e9f3f9c1d6c1e4d1d","title":"this is my new update","description":"this is my new description","completed":true,"user":"5f7f8b1e9f3f9c1d6c1e4d1e","createdAt":"2024-02-08T16:10:08.998Z","updatedAt":"2024-02-09T17:45:25.902Z","__v":0}}

==DELETE:==

curl -i -b sessions.txt -X DELETE http://10.2.0.238:8003/nodetodo/todos/delete/65cf94a7cabbdb52f57120ac

==output:==

{"httpStatusCode":200,"resultMessage":"Todo Deleted Successfully"}



================================================================
DEPRECATED
================================================================


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


