
1. Post Request:

curl -X POST http://localhost:8001/account -H "Content-Type: application/json" -d '{"firstName": "Herbert", "lastName": "Tamayo"}'

![[Pasted image 20230704112848.png]]

![[Pasted image 20230704112927.png]]

![[Pasted image 20230704113132.png]]


2. Get Request:
curl -X GET http://localhost:8001/accounts

![[Pasted image 20230704170809.png]]

Get Request con Token que se genera en el POST:

curl -i --header "x-jwt-token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJFeHBpcmVzQXQiOjE1MDAwLCJhY2NvdW50TnVtYmVyIjo3Mjc4ODd9.n1JxNW3jLNJGLv23rci9jxkCJW-fM8EgnMtjcvpAscg" -X GET http://localhost:8001/account/9

![[Pasted image 20230808142148.png]]


3. Delete Request:
curl -X DELETE http://localhost:8001/account/2

![[Pasted image 20230719142728.png]]

4. Login Request:
curl -X POST http://localhost:8001/login -H "Content-Type: application/json" -d '{"number": 498081, "password": "MJ123"}'

![[Pasted image 20230831144443.png]]

![[Pasted image 20230831144515.png]]

5. Login Request cuando ya el password esta encriptado:

![[Pasted image 20230906142717.png]]

![[Pasted image 20230906142803.png]]

6. 