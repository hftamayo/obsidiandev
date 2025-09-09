### getByID & newTask:

- Return a single task object in data
- No etag or lastModified in the response

### getAll:

- Returns tasks array + pagination + metadata in data
- Has etag and lastModified in the data object

## 🎯 The Issue

Your API has inconsistent response structures:
1. Single task operations (getByID, newTask, updateTask, etc.) return just the task
2. List operations (getAll) return tasks + pagination + metadata


======BUG

==la version 0.1.5-experimental hace referencia a goresttodo que cuenta con implementacion de rate limiting, caching con Redis y logError centralizado; estas consideraciones deben tenerse en cuenta para las versiones 0.1.4-main y 0.1.4-experimental que son nodetodo y jsbtodo:

Cuando estaba haciendo testing me di cuenta que se necesita refactoring en estos aspectos:

## Current Issues
Cache Dependency: Every handler method is tightly coupled to Redis cache operations
Error Logging Coupling: Every error path requires logging, making testing complex
Redis Client Dependency: Direct Redis client usage throughout the codebase

## Benefits of This Refactoring
Testability: Each component can be tested in isolation
Maintainability: Clear separation of concerns
Flexibility: Easy to swap implementations (Redis → Memory, etc.)
Performance: Cache operations can be non-blocking
Reliability: Error logging doesn't affect main flow

## Implementation Priority
Phase 1: Extract interfaces and create mock implementations
Phase 2: Refactor handlers to use dependency injection
Phase 3: Move cache operations to service layer
Phase 4: Make cache operations non-blocking
Phase 5: Simplify tests


## Ejemplo sobre cómo fue planificada y ejecutada este refactor:

## Refactoring Strategy

### 1. **Make Error Logging Optional/Non-blocking**
```go
// Instead of blocking error logging
h.errorLogService.LogError("Task_create", err)

// Make it non-blocking
go func() {
    if err := h.errorLogService.LogError("Task_create", err); err != nil {
        // Only log if logging itself fails (rare)
        log.Printf("Failed to log error: %v", err)
    }
}()
```

### 2. **Reduce Error Paths**
```go
// Instead of logging every cache operation failure
if err := h.cache.Set(key, value, ttl); err != nil {
    h.errorLogService.LogError("Task_cache_set", err) // Remove this
}

// Just handle cache failures gracefully
if err := h.cache.Set(key, value, ttl); err != nil {
    // Cache failure shouldn't break the main flow
    // Maybe log at debug level or just continue
}
```

### 3. **Separate Concerns**
```go
// Move cache operations to service layer
type TaskService struct {
    repo   TaskRepository
    cache  CacheService
    logger ErrorLogger
}

func (s *TaskService) Create(task *models.Task) (*models.Task, error) {
    // Business logic here
    createdTask, err := s.repo.Create(task)
    if err != nil {
        return nil, err
    }
    
    // Cache operations (non-blocking, non-critical)
    go s.cache.Set(fmt.Sprintf("task:%d", createdTask.ID), createdTask, time.Hour)
    
    return createdTask, nil
}
```

## Files to be Updated

### **Priority 1: Core Handler Files**
1. **`api/v1/task/task_handler.go`** - Main refactoring target
2. **`api/v1/task/task_service.go`** - Move cache operations here
3. **`api/v1/task/task_service_interface.go`** - Update interface

### **Priority 2: Configuration Files**
4. **`pkg/config/errorlog.go`** - Add non-blocking logging option
5. **`pkg/config/cache.go`** - Improve cache configuration

### **Priority 3: Test Files (After Refactoring)**
6. **`api/v1/task/task_handler_test.go`** - Simplify tests (no more complex error logging expectations)
7. **`api/v1/task/task_service_test.go`** - New service layer tests
8. **`pkg/config/errorlog_test.go`** - Fix the broken test

### **Priority 4: Additional Files**
9. **`api/v1/task/task_dto.go`** - May need updates for new response structures
10. **`api/v1/task/constants.go`** - Cache key constants

## Recommended Order

### **Phase 1: Foundation**
1. `pkg/config/errorlog.go` - Add non-blocking logging
2. `pkg/config/cache.go` - Improve cache config

### **Phase 2: Service Layer**
3. `api/v1/task/task_service_interface.go` - Update interface
4. `api/v1/task/task_service.go` - Move cache operations here

### **Phase 3: Handler Refactoring**
5. `api/v1/task/task_handler.go` - Simplify error handling

### **Phase 4: Testing**
6. `api/v1/task/task_handler_test.go` - Simplify tests
7. `api/v1/task/task_service_test.go` - New service tests
8. `pkg/config/errorlog_test.go` - Fix broken test

## Expected Benefits

1. **Simpler Tests** - No more complex error logging expectations
2. **Better Performance** - Non-blocking error logging
3. **Cleaner Code** - Separation of concerns
4. **More Reliable** - Cache failures don't break main flow
5. **Easier Maintenance** - Clear responsibilities

