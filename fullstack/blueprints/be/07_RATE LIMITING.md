
## API Gateway rate limiting
The API Gateway is usually responsible for edge-level rate limiting.
That means it protects the whole platform from abusive external traffic

Examples:
```
Limit anonymous IPs to 100 requests/minute
Limit authenticated users to 1000 requests/minute
Limit API keys by subscription plan
Limit /login harder than /companies
Protect all microservices consistently
```

The gateway is great for:
centralized policies
cross-service limits
API key/client-level limits
tenant-level limits
IP-based limits
global throttling
rejecting abusive traffic before it reaches your app
consistent 429 Too Many Requests behavior

## Application-level rate limiting

Use cases:

```
This endpoint is expensive because it triggers reports.
This endpoint calls an external provider.
This endpoint must be limited per business role.
This operation should consume 5 tokens instead of 1.
This internal endpoint needs service-specific protection.
This endpoint should have a lower limit because it hits the DB hard.
```

The application has better context about:
domain rules
roles
users
endpoint cost
business operations
internal service behavior
expensive database paths
external provider calls
So app-level rate limiting is not a mistake. It’s a defense-in-depth mechanism.

## Final setup:

API Gateway:
  broad, external, centralized limits

Application:
  fine-grained, domain-aware limits


Examples:
```
Gateway:
  /api/** -> 1000 req/min per user
  /api/auth/login -> 20 req/min per IP
  /api/admin/** -> 300 req/min per user

Application:
  generate monthly absence report -> 5 req/min per user
  bulk company import -> 2 req/min per supervisor
  expensive search endpoint -> 30 req/min per role
```


## SUMMARY

1. Api Gateway is in charge of broad limits:

IP limits
user limits
tenant limits
API key limits
path group limits


Gateway headers:
X-RateLimit-Layer: gateway
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 0
Retry-After: 60

2. Application is in charge of service specific usage

role-aware limits
expensive endpoints
business-sensitive operations
downstream-protection limits

Application headers:
X-RateLimit-Layer: application
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 0
Retry-After: 60

### Common keys for rate limiting

IP address
user id
role
tenant id
API key/client id
endpoint path
HTTP method + path

### Keys delegated to gateway:

IP
API key
JWT subject
tenant
route

### Keys delegated to app:

user id
role
tenant
business operation
endpoint

