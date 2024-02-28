fabuloso articulo sobre el patron controller-service-repository y ejemplos de  unit testing:
https://tom-collings.medium.com/controller-service-repository-16e29a4684e5

excelente articulo sobre unit testing: https://www.twilio.com/blog/java-junit-effective-unit-tests

https://www.toptal.com/java/unit-integration-junit-tests

Article for Test cases names and best practices: https://www.baeldung.com/java-unit-testing-best-practices

In a [summery for tests](https://gist.github.com/wojteklu/73c6914cc446146b8b533c0988cf8d29#tests) of the book Clean Code by Robert C. Martin the following points are important

> - One assert per test.
> - Readable.
> - Fast.
> - Independent.
> - Repeatable.

TESTING GUIDELINES DEL MODEL, CONTROLLER, REPO Y SERVICE:
https://blog.devgenius.io/spring-boot-deep-dive-on-unit-testing-92bbdf549594
https://www.freecodecamp.org/news/java-unit-testing/
https://semaphoreci.com/community/tutorials/stubbing-and-mocking-with-mockito-2-and-junit

======================
TEST DEL MODEL:
======================
como testear date and time: https://www.baeldung.com/java-override-system-time



======================
TEST DEL CONTROLLER:
=========================
Testing del controller using Mockito: https://howtodoinjava.com/spring-boot2/testing/rest-controller-unit-test-example/

=======================
TEST DEL REPOSITORY:
=======================

https://github.com/sajedul-karim/SpringReadyApp/blob/master/src/test/java/com/appcoder/springreadyapp/CustomerRepositoryTest.java

=======================
TEST DEL SERVICE:
========================



=============================
SCRIPTS PARA TESTING DE LOS ENDPOINTS
==============================

TESTING DE LOS ENDPOINTS DE AUTHENTICACION:
=
==Register:

curl -i -X POST http://localhost:8002/api/auth/register -H "Content-Type: application/json" -d '{"name":"Herbert Tamayo", "email":"hftamayo@gmail.com", "password": "Java123", "age": 25, "isAdmin": true, "status": true}'

![[Pasted image 20240205144135.png]]

notese los bugs en rojo los cuales deben corregirse

==Login:

curl  --cookie-jar jsb.txt -i -X POST http://localhost:8002/api/auth/login -H "Content-Type: application/json" -d '{ "email":"sebas@gmail.com", "password": "password123"}'



* Todo Model:


curl -X GET  http://localhost:8080/tasks/

cuando no hay registros:

![[Pasted image 20230719153536.png]]

cuando ya hay datos:

![[Pasted image 20230719154618.png]]

curl -X POST http://localhost:8080/savetask -H "Content-Type: application/json" -d '{"title": "go to the supermarket", "description": "cheese, fruits, veggies"}'

![[Pasted image 20230719154505.png]]

curl -X PUT http://localhost:8080/updatetask/2 -H "Content-Type: application/json" -d '{"title": "pay the bills", "description": "school, electricity, mobile phone, internet"}'

![[Pasted image 20230721152403.png]]


curl -X DELETE http://localhost:8080/deletetask/3


![[Pasted image 20230719154956.png]]

TESTING DE USUARIOS:
=

curl -X POST http://localhost:8002/users/saveuser -H "Content-Type: application/json" -d '{"name":"Herbert Tamayo", "email":"hftamayo@gmail.com", "password": "Java123", "age": 25, "isAdmin": true, "status": true}'


