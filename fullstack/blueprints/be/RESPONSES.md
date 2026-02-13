
## Command Operations:

### Adding a Record:

```
curl -i -X POST "http://localhost:8081/api/v1/companies" -H "Content-Type: application/json" -d '{"name" : "Acme4 Subsidiary4", "description": "Company created via curl smoke test4", "address": "400 Market Street"}'

Error Response:
HTTP/1.1 409 
Content-Type: application/json
Transfer-Encoding: chunked
Date: Wed, 11 Feb 2026 14:37:37 GMT

{"responseType":"error","statusCode":409,"resultMessage":"ENTITY_ALREADY_EXISTS","data":null,"timestamp":"2026-02-11T14:37:37.092669268Z","cacheTTL":null}

Success Response:
HTTP/1.1 201 
Content-Type: application/json
Transfer-Encoding: chunked
Date: Wed, 11 Feb 2026 14:38:24 GMT

{"responseType":"success","statusCode":201,"resultMessage":"ENTITY_CREATED","data":{"id":7,"name":"Acme4 Subsidiary4","description":"Company created via curl smoke test4","address":"400 Market Street","createdBy":0,"updatedBy":0,"createdDate":"2026-02-11T14:38:24.152194Z","updatedDate":"2026-02-11T14:38:24.152225Z","deleted":false,"active":true},"timestamp":"2026-02-11T14:38:24.213800380Z","cacheTTL":null}

```

### Deactivating a Record

```
curl -i -X PUT "http://localhost:8081/api/v1/companies/5/deactivate"

Success:
HTTP/1.1 200 
Content-Type: application/json
Transfer-Encoding: chunked
Date: Wed, 11 Feb 2026 14:39:10 GMT

{"responseType":"success","statusCode":200,"resultMessage":"ENTITY_UPDATED","data":{"id":5,"name":"Acme2 Subsidiary2","description":"Company2 created via curl smoke test","address":"200 Market Street","createdBy":null,"updatedBy":null,"createdDate":null,"updatedDate":null,"deleted":false,"active":false},"timestamp":"2026-02-11T14:39:10.594526465Z","cacheTTL":null}

HTTP/1.1 404 
Content-Type: application/json
Transfer-Encoding: chunked
Date: Wed, 11 Feb 2026 14:40:48 GMT

{"responseType":"error","statusCode":404,"resultMessage":"ENTITY_NOT_FOUND","data":null,"timestamp":"2026-02-11T14:40:48.483167493Z","cacheTTL":null}
```

### Activating a Record

```
curl -i -X PUT "http://localhost:8081/api/v1/companies/5/activate"
Success:
HTTP/1.1 200 
Content-Type: application/json
Transfer-Encoding: chunked
Date: Wed, 11 Feb 2026 18:01:14 GMT

{"responseType":"success","statusCode":200,"resultMessage":"ENTITY_UPDATED","data":{"id":3,"name":"Initech","description":"Internal seed record","address":"789 Industrial Rd","createdBy":null,"updatedBy":null,"createdDate":null,"updatedDate":null,"deleted":false,"active":true},"timestamp":"2026-02-11T18:01:14.354750919Z","cacheTTL":null}

Error:
HTTP/1.1 404 
Content-Type: application/json
Transfer-Encoding: chunked
Date: Wed, 11 Feb 2026 18:01:43 GMT

{"responseType":"error","statusCode":404,"resultMessage":"ENTITY_NOT_FOUND","data":null,"timestamp":"2026-02-11T18:01:43.688788944Z","cacheTTL":null}
```

### Updating a Record

```
curl -i -X PUT "http://localhost:8081/api/v1/companies/4" -H "Content-Type: application/json"   -d '{
"name": "Acme9 Subsidiary9",
"description": "UPDATED TEST",
"address": "900 Market Street"
 }'

Success:
HTTP/1.1 200 
Content-Type: application/json
Transfer-Encoding: chunked
Date: Wed, 11 Feb 2026 14:43:53 GMT

{"responseType":"success","statusCode":200,"resultMessage":"ENTITY_UPDATED","data":{"id":4,"name":"Acme9 Subsidiary9","description":"UPDATED TEST","address":"900 Market Street","createdBy":null,"updatedBy":null,"createdDate":null,"updatedDate":null,"deleted":false,"active":false},"timestamp":"2026-02-11T14:43:53.381466863Z","cacheTTL":null}


Error:
HTTP/1.1 404 
Content-Type: application/json
Transfer-Encoding: chunked
Date: Wed, 11 Feb 2026 14:44:33 GMT

{"responseType":"error","statusCode":404,"resultMessage":"ENTITY_NOT_FOUND","data":null,"timestamp":"2026-02-11T14:44:33.877399750Z","cacheTTL":null}
```

### Deleting a Record

```
curl -i -X DELETE "http://localhost:8081/api/v1/companies/5"
Error:
HTTP/1.1 400 
Content-Type: application/json
Transfer-Encoding: chunked
Date: Wed, 11 Feb 2026 17:44:03 GMT
Connection: close

{"responseType":"error","statusCode":400,"resultMessage":"BUSINESS_LOGIC_VIOLATION","data":null,"timestamp":"2026-02-11T17:44:03.698663326Z","cacheTTL":null}

Success:
HTTP/1.1 200 
Content-Type: application/json
Transfer-Encoding: chunked
Date: Wed, 11 Feb 2026 17:44:29 GMT

{"responseType":"success","statusCode":200,"resultMessage":"ENTITY_DELETED","data":{"id":2,"name":"Globex","description":"Reference company for testing","address":"456 Market Ave","active":false,"deleted":true,"auditInfo":{"createdBy":null,"updatedBy":null,"createdDate":null,"updatedDate":null,"present":false},"updatedBy":null,"createdBy":null,"createdDate":null,"updatedDate":null},"timestamp":"2026-02-11T17:44:28.979215565Z","cacheTTL":null}
```


### Restoring a record

```
curl -i -X PUT "http://localhost:8081/api/v1/companies/5/restore"
Success:
HTTP/1.1 200 
Content-Type: application/json
Transfer-Encoding: chunked
Date: Wed, 11 Feb 2026 17:49:15 GMT

{"responseType":"success","statusCode":200,"resultMessage":"ENTITY_UPDATED","data":{"id":5,"name":"Acme2 Subsidiary2","description":"Company2 created via curl smoke test","address":"200 Market Street","createdBy":null,"updatedBy":null,"createdDate":null,"updatedDate":null,"deleted":false,"active":true},"timestamp":"2026-02-11T17:49:15.230796872Z","cacheTTL":null}

Error:
HTTP/1.1 404 
Content-Type: application/json
Transfer-Encoding: chunked
Date: Wed, 11 Feb 2026 17:49:53 GMT

{"responseType":"error","statusCode":404,"resultMessage":"ENTITY_NOT_FOUND","data":null,"timestamp":"2026-02-11T17:49:53.192936684Z","cacheTTL":null}

```

