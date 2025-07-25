
==get all==;

```
{
    "code": 200,
    "resultMessage": "OPERATION_SUCCESS",
    "data": {
        "tasks": [
            {
                "id": 82,
                "title": "chibolita",
                "description": "",
                "done": false,
                "owner": 1,
                "createdAt": "2025-07-11T20:56:10.822611Z",
                "updatedAt": "2025-07-11T20:56:10.822611Z"
            },
            {
                "id": 81,
                "title": "make some noise",
                "description": "",
                "done": true,
                "owner": 1,
                "createdAt": "2025-07-09T15:07:14.763106Z",
                "updatedAt": "2025-07-11T20:56:28.586982Z"
            },
            {
                "id": 80,
                "title": "Dr. Dre",
                "description": "",
                "done": false,
                "owner": 1,
                "createdAt": "2025-07-09T14:58:51.988737Z",
                "updatedAt": "2025-07-09T14:58:51.988737Z"
            },
            {
                "id": 78,
                "title": "Andinosaurio",
                "description": "Andi Andrea",
                "done": true,
                "owner": 1,
                "createdAt": "2025-07-09T14:58:26.291628Z",
                "updatedAt": "2025-07-11T15:19:55.479607Z"
            },
            {
                "id": 77,
                "title": "pequeño bilucito",
                "description": "",
                "done": false,
                "owner": 1,
                "createdAt": "2025-07-03T14:44:29.735497Z",
                "updatedAt": "2025-07-03T14:44:29.735497Z"
            }
        ],
        "pagination": {
            "nextCursor": "NzU6LTYyMTM1NTk2ODAwOg==",
            "prevCursor": "ODI6LTYyMTM1NTk2ODAwOg==",
            "limit": 5,
            "totalCount": 37,
            "hasMore": true,
            "currentPage": 1,
            "totalPages": 8,
            "order": "desc",
            "hasPrev": true,
            "isFirstPage": false,
            "isLastPage": false
        },
        "etag": "W/\"b2ac5dce55ea7f24bc79cb8d6b2c7b52a5b143ae38f8883fef8bc9ed180b8651\"",
        "lastModified": "Wed, 16 Jul 2025 15:19:11 GMT"
    },
    "timestamp": 1752679151,
    "cacheTTL": 30
}
```


==getByID==;

```
{
    "code": 200,
    "resultMessage": "OPERATION_SUCCESS",
    "data": {
        "id": 76,
        "title": "masinko masinko",
        "description": "pequeño bilucito",
        "done": true,
        "owner": 1,
        "createdAt": "2025-07-03T14:44:16.987735Z",
        "updatedAt": "2025-07-11T15:19:59.807816Z"
    },
    "timestamp": 1752679212,
    "cacheTTL": 30
}
```


==newTask==:

```
{
    "code": 201,
    "resultMessage": "OPERATION_SUCCESS",
    "data": {
        "id": 83,
        "title": "Go to Evanescence tribute",
        "description": "",
        "done": false,
        "owner": 1,
        "createdAt": "2025-07-16T15:24:29.463492325Z",
        "updatedAt": "2025-07-16T15:24:29.463492325Z"
    },
    "timestamp": 1752679469,
    "cacheTTL": 30
}
```

==update a Task==:

```
{
    "code": 200,
    "resultMessage": "OPERATION_SUCCESS",
    "data": {
        "id": 83,
        "title": "veneno",
        "description": "go premium please",
        "done": false,
        "owner": 1,
        "createdAt": "2025-07-16T15:24:29.463492Z",
        "updatedAt": "2025-07-16T15:35:52.878502Z"
    },
    "timestamp": 1752680152,
    "cacheTTL": 30
}
```

==delete a task==;

```
{
    "code": 200,
    "resultMessage": "OPERATION_SUCCESS",
    "timestamp": 1752680214
}
```