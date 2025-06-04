
### Version 0.3.6
- fecha: 06-04-2025
- caracteristicas:
	- rate limiting
		- Added operation-based rate limits (different limits for read vs write)
		- Implemented higher limits for prefetch operations (200/min)
		- Added proper Retry-After headers for rate limit responses
		- Used Redis for distributed rate limiting
		- Added prefetch detection via headers and query parameters

	- cache validation/invalidation
		- Implemented tag-based cache invalidation for more precise cache control
		- Added immediate cache invalidation after write operations
		- Fixed issues with stale data appearing in list views
		- Added cache-busting with `_t` query parameter
		- Added small delays after DB operations to ensure consistency
		- Reduced TTL from 60 to 30 seconds for faster development testing


- Future improvements:
	##### 1.edge cases:
	- **Stale Cache After Service Restart**: Ensure Redis TTL values are reasonable to prevent very old data from being served
	- **Cache Stampede**: When cache expires, multiple requests might try to rebuild it simultaneously - implement singleflight pattern
	- **Cache Inconsistency**: If DB operations fail after cache invalidation, implement two-phase commit for critical operations
	- **Cache Size Limits**: Add cache eviction policies to prevent memory issues with large datasets
	- **Database Timeouts**: Add context with timeouts for all database operations
	- **Partial Updates**: Ensure transactions are used for operations that modify multiple records
	- **Race Conditions**: Implement optimistic locking for concurrent updates to the same record
	- **Connection Pool Exhaustion**: Set reasonable pool sizes and handle connection limits
	- **Malformed JSON**: Handle JSON parse errors gracefully
	- **Invalid UTF-8**: Ensure proper encoding handling for international text
	- **XSS Protection**: Sanitize inputs that might be displayed in UI
	- **Excessively Large Inputs**: Limit request body sizes and pagination parameters
	- **Token Expiration**: Gracefully handle expired tokens with clear error messages
	- **Rate Limit Bypass Attempts**: Check for distributed attacks across multiple IPs
	- **Permission Boundary Cases**: Verify edge cases where users might access resources they shouldn't
	- **Graceful Service Degradation**: When Redis or other dependencies are down, degrade gracefully
	- **Request Timeout Handling**: Add request-level timeouts to prevent hanging connections
	- **Health Check Endpoints**: Skip rate limiting and authentication for health checks
	- **Logging Overflow**: Protect against log flooding during error cascades
	- **Deadlocks**: Ensure consistent lock acquisition order across your codebase
	- **Fan-out Overload**: When spawning goroutines, use worker pools to limit concurrency
	- **Context Propagation**: Ensure context cancellation propagates through all operations
	- - **Handling Incomplete Downloads**: Include Content-Length headers and ETag validation
	- **Browser Cache Inconsistencies**: Test cache headers across different browsers
	- **Mobile Network Transitions**: Support resumable operations for mobile clients

	##### 2. Observability & Monitoring
	- Add structured logging for cache hits/misses
	- Implement metrics for cache effectiveness and rate limit hits
	- Add tracing for request flows through the system
	
	##### 3. Error Handling Improvements
	- Create consistent error response formats
	- Add correlation IDs for error tracking
	- Implement better validation error messages
	
	##### 4. Performance Optimizations
	- Add support for HTTP/2
	- Implement database query optimization
	- Consider adding background refresh for frequently accessed data
	
	##### 5. Security Enhancements
	- Add JWT validation
	- Implement role-based access control
	- Add request validation to prevent injection attacks

