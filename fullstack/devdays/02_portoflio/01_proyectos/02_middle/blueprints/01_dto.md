
1. cada entidad debe tener su propio dto, inclusive paginacion, error
2. una sola clase envelope para gestionar respuestas (success, error)
3. la estructura de respuesta es estandar:

{
    "code": 200,
    "resultMessage": "OPERATION_SUCCESS",
    "data": {
	    "timestamp": "2025-07-16T15:18:20Z",
	    "connectionTime": 0.000461575,
	    "databaseStatus": "healthy"
    },
    "dataList":{
		null
    },
}

4. Los controllers son los que disparan la interaccion con externos (frontend, servicios) por medio de la DTO envelope
5. Los servicesImpl envian respuesta via Dtos y CustomException sin usar el DTO envelope
6. tiene que haber una utileria encargda de armar la respuesta, ya sea success o error