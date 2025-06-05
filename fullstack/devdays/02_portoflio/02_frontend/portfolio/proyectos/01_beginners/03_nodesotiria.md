## Techical sheet:

## Core Technology Stack

|Category|Technologies|
|---|---|
|**Language**|TypeScript 5.x|
|**Runtime**|Node.js 20.x|
|**Framework**|Express.js 4.x|
|**Database**|PostgreSQL 15.x|
|**ORM**|Prisma 6.x|
|**Validation**|express-validator|
|**Configuration**|dotenv|
|**CORS**|cors middleware|
|**Parsing**|cookie-parser, express.json|

## Architecture Patterns

|Pattern|Implementation|
|---|---|
|**CQRS**|Command and Query Responsibility Segregation pattern with separate services for commands (write operations) and queries (read operations)|
|**Decorator**|TypeScript decorators for cross-cutting concerns like logging|
|**Singleton**|Environment configuration using the Singleton pattern|
|**Repository**|Prisma client as the data access layer|
|**DTO**|Data Transfer Objects for API request/response standardization|
|**Middleware**|Express middleware for validation, error handling, and other cross-cutting concerns|

For reference, we completed:

1. ✅ Environment configuration
2. ✅ Database setup with Prisma
3. ✅ Health check endpoints
4. ✅ Request validation

Next session we'll focus on:

- Global error handling
- Custom error classes
- Error logging integration
- Standardized error responses

