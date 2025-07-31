

Request → RateLimiterAspect → Check Bucket → Allow/Reject → Controller
                                    ↓
                            RateLimiterUtil (Bucket4j)
                                    ↓
                            Envelope Error Response


### Security Chain Order

1. CORS Filter
2. Rate Limiting Filter ← NEW
3. Authentication Filter
4. Authorization Filter

### Integration Points

#### AuthenticationFilter Integration

- Check rate limits before token validation

- Add rate limit headers to successful responses

- Handle rate limit errors with envelope pattern

#### Global Exception Handler

- Handle RateLimiterException with 429 status

- Format rate limit errors using ErrorResponseDto

- Add rate limit headers to error responses

### Configuration Updates

- Rate limit properties in application.yml

- Security-specific rate limits (login attempts, token validation)

- User-specific rate limits (premium vs free users)

The security package needs updates to integrate rate limiting seamlessly!