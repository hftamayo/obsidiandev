
==caracteristicas generales:
- good security
- it’s maintainable 
- it’s scalable 
- it’s deployable 

### Caching, headers and Error loging

## 1. Pagination Support

- **Offset-Based Pagination**: Support for `page` and `limit` parameters
- **Pagination Metadata**: Response should include:
    - `totalCount`: Total number of items across all pages
    - `totalPages`: Number of available pages
    - `currentPage`: Current page number
    - `limit`: Items per page
- **Example Endpoint**: `GET /tasks/task/list/page?page=1&limit=5`

##  2. Rate Limiting

- **Reasonable Limits**: Higher limits for read operations, lower for writes
- **Proper Headers**: Include standard rate limit headers:
    
    X-RateLimit-Limit: 100
    
    X-RateLimit-Remaining: 99
    
    X-RateLimit-Reset: 1623460800
     
	- **Retry-After Header**: When rate limited, include a `Retry-After` header
	- **Consistent 429 Responses**: Use standard 429 status with clear error messages


## 3. Caching Support

- **Cache Control Headers**: Proper `Cache-Control` headers on responses:
    
    Cache-Control: max-age=60, stale-while-revalidate=300
      
	- **ETags**: Support conditional requests with ETags
	- **Cache Invalidation**: Invalidate related cache entries after mutations
	- **Cache TTL**: Short TTLs during development (10-30s), longer in production

## 4. CRUD Operations

- **Standard Endpoints**:
    - `GET /tasks/task/list/page` - List tasks with pagination
    - `GET /tasks/task/{id}` - Get a specific task
    - `POST /tasks/task` - Create a new task
    - `PUT /tasks/task` - Update a task
    - `PATCH /tasks/task/{id}/toggle` - Toggle task completion status
    - `DELETE /tasks/task/{id}` - Delete a task

## 5. Data Structure

- **Consistent Response Format**:
    
    {
  "code": 200,
  "resultMessage": "OPERATION_SUCCESS",
  "data": {
    "tasks": [...],
    "pagination": {
      "page": 1,
      "limit": 5,
      "totalCount": 20,
      "totalPages": 4
    }
  },
  "timestamp": 1623460800,
  "cacheTTL": 60
}


## 6. Error Handling

- **Consistent Error Format**:
    
{
  "code": 400,
  "resultMessage": "VALIDATION_ERROR",
  "errors": [
    { "field": "title", "message": "Title is required" }
  ]
}

- **Appropriate Status Codes**: 400, 401, 403, 404, 429, 500


### ==Detalles:

1. **Type Safety for Cache Operations**
   - Added proper type assertions with error handling
   - Protected against runtime panics from invalid cached data

2. **Error Logging for Cache Operations**
   - Added error logging for all cache set/delete operations
   - Maintained consistent log naming conventions

3. **Support for Weak ETags**
   - Added support for both strong and weak ETags
   - Improved compatibility with different clients and proxies

4. **Consistent Headers**
   - Unified approach to setting cache headers
   - Helper functions for setting ETag and cache control headers

5. **Proper Cache Invalidation**
   - Invalidating all relevant cache keys on write operations
   - Ensuring data consistency across endpoints

6. **Consistent Error Handling**: Well-structured error responses with appropriate HTTP status codes

7. **Robust Validation**: Input validation with useful error messages

8. **Efficient Caching**: Optimized cache strategy including TTL and invalidation

9. **Clean Headers**: Proper HTTP headers for caching, ETags, and Last-Modified

10.  **Maintainable Structure**: Consistent handler structure across all endpoints

11. Content negotiation

12. Conditional requests (If-None-Match)

13. - Proper caching

14. - Pagination (both cursor and page-based)



### Race Conditions

1. escenarios donde se explota race conditions: restablecer password y cuentas, OTP y login workflow

2. 

## 7. Performance Considerations

- **Query Optimization**: Efficient database queries for pagination
- **Index Support**: Proper indexing for fields used in sorting/filtering
- **Sorting**: Default sorting by creation date (newest first)
- **Selective Fields**: Support for returning only needed fields

## 8. Advanced Features

- **Filtering**: Support `status=completed` or `status=pending` filters
- **Search**: Text search across task titles and descriptions
- **Batch Operations**: Endpoints for batch updates/deletes
- **Websocket Notifications**: Real-time updates for collaborative features