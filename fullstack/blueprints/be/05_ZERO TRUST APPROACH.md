
So the real zero-trust approach is:

1. **Validate every external input**
2. **Reject unknown / extra fields**
3. **Normalize data**
4. **Enforce authorization on every protected action**
5. **Never trust frontend decisions**

### 1. Enable method-level validation for path/query parameters

Right now, on only works reliably when the controller is annotated with `@Validated`. `@Positive``@PathVariable`

### 2. Centralize validation error handling

Without a global exception handler, validation errors may come back inconsistently.

### 3. Reject unknown JSON properties

This is very important for zero trust. If the frontend sends extra fields you didn’t define, the backend should reject them.

Example attack-ish payload:

{
  "name": "ACME",
  "description": "desc",
  "address": "street 1",
  "isAdmin": true,
  "deleted": false
}


Even if your DTO ignores these today, a zero-trust backend should say: **“Nope. Unknown field.”**

### 4. Tighten DTO constraints

+ is good, but you can also: `@NotBlank``@Size`

- restrict allowed characters with `@Pattern`
- ensure no `null`
- validate nested objects if added later

### 5. Keep authorization separate

For endpoints like activate/deactivate/delete/update, you still need server-side permission checks. Validation alone won’t stop a user from modifying a resource they shouldn’t access if authorization is weak.

----
# Best-practice zero-trust checklist for your backend

## Request boundary

- on every body DTO `@Valid`
- `@Validated` on controllers/services validating method params
- , , , `@Pattern` as needed `@Positive``@NotBlank``@Size`
- reject unknown JSON fields
- reject malformed JSON

## Business boundary

- never trust frontend-controlled IDs, flags, roles, ownership, or status
- derive sensitive values server-side
- validate state transitions in the domain/application layer

## Access control boundary

- check authorization for every action
- check ownership / tenant scope / role scope server-side
- never rely on hidden frontend buttons as security