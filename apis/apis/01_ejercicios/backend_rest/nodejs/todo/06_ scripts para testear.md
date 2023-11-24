br
* User model:

curl -i -X POST http://localhost:8003/nodetodo/users/register -H "Content-Type: application/json" -d '{"name": "Milucito", "email": "tester30@martinez.com", "password": "milucito", "age": "4"}'


curl --cookie-jar sessions.txt -i -X POST http://localhost:8003/nodetodo/users/login -H "Content-Type: application/json" -d '{"email": "tester30@martinez.com", "password":"milucito"}'


curl -i -b sessions.txt -X GET http://localhost:8003/nodetodo/users/me


curl -i -b sessions.txt -X GET http://localhost:8003/nodetodo/users/logout

curl -i -b sessions.txt -X PUT http://localhost:5000/nodetodo/users/updatepassword -H "Content-Type: application/json" -d '{"password": "milucito", "newPassword": "milucitochivo"}'

curl -i -b sessions.txt -X PUT http://localhost:5000/nodetodo/users/updatedetails -H "Content-Type: application/json" -d  '{"name": "herbert tamayo", "email": "hftamayo@gmail.com", "age": "35"}'


curl -i -b sessions.txt -X DELETE http://localhost:5000/nodetodo/users/deleteuser


*  ToDo Model

curl -X POST http://localhost:5000/api/todos/create -H "Content-Type: application/json" -d '{"title": "go to the supermarket", "description": "cheese, fruits, veggies"}'

curl http://localhost:5000/api/todos/

user: 6526e11d3c4440af560daee8
task: 6526e3213c4440af560daeee

curl http://localhost:5000/api/todos/63xcff...

curl -X PUT http://localhost:5000/api/todos/update/63xcff... -H "Content-Type: application/json" -d '{"title": "my updated title", "description": "my updated description", "completed": "true"}'

curl -X DELETE http://localhost:5000/api/todos/delete/63xcff... 