
Paso 1: codear todo lo que no requiere testing en la rama main: entidades, constantes, objetos DTO, objetos mapper, objetos interfaces

Paso 2: crear scaffolds de los metodos de serviceImpl y controllers, es valido que devuelvan null

Paso 3: seleccionar un flujo, en mi caso empece con save

Paso 4: escribir los unit tests del serviceImpl y del controller del flujo seleccionado

Paso 5: correr los tests

Paso 6: codear en la rama experimental

Paso 7: replicar a unstable, correr los tests y ajustar

Paso 8: los unit tests deben ser congruentes con los edge cases y con las clases error gestionadas mediante el patron Result: entidad duplicada, entidad not found, input validation error, business logic error y unknown exception

Paso 9: seguir ese mismo patron para el siguiente flujo, puede ser update pues se parece a save.