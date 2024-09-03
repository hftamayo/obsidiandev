
==Test Driven Development

To determine if your project follows the Test-Driven Development (TDD) approach, consider the following criteria:

1. **Test First**: In TDD, tests are written before the actual code. Ensure that you have written tests before implementing the corresponding functionality.
2. **Incremental Development**: Code is developed in small increments, with each increment starting with a failing test.
3. **Refactoring**: After making a test pass, the code is refactored to improve its structure and readability while ensuring that all tests still pass.

If your project meets these criteria, you can say it follows the TDD approach. Having a complete set of unit tests is a good indicator, but the process of writing tests first and developing incrementally is crucial to TDD.


==Domain Driven Development

Domain-Driven Development (DDD) is a software development approach that focuses on modeling the domain of the application and aligning the software design with the business needs. Here are some key concepts and steps to follow DDD:

### Key Concepts:
1. **Domain**: The subject area to which the user applies a program is the domain.
2. **Entities**: Objects that have a distinct identity that runs through time and different states.
3. **Value Objects**: Objects that describe some characteristics or attributes but have no conceptual identity.
4. **Aggregates**: A cluster of domain objects that can be treated as a single unit.
5. **Repositories**: Mechanisms for encapsulating storage, retrieval, and search behavior which emulates a collection of objects.
6. **Services**: Operations or actions that don't naturally fit within the entities or value objects.
7. **Factories**: Objects that are responsible for creating other objects.
8. **Bounded Context**: A boundary within which a particular model is defined and applicable.

### Steps to Implement DDD:
1. **Identify the Core Domain and Subdomains**: Understand the business and identify the core domain and subdomains.
2. **Define Bounded Contexts**: Define clear boundaries for each subdomain.
3. **Create Domain Models**: Develop models for each bounded context.
4. **Implement Repositories and Services**: Create repositories for data access and services for business logic.
5. **Use Factories for Object Creation**: Use factories to handle the creation of complex objects.

### Example Implementation:

#### 1. Entity
```java
package com.hftamayo.java.todo.domain;

public class TodoItem {
    private Long id;
    private String description;
    private boolean completed;

    // Getters and Setters
}
```

#### 2. Value Object
```java
package com.hftamayo.java.todo.domain;

public class TodoDescription {
    private final String description;

    public TodoDescription(String description) {
        this.description = description;
    }

    public String getDescription() {
        return description;
    }
}
```

#### 3. Repository
```java
package com.hftamayo.java.todo.repository;

import com.hftamayo.java.todo.domain.TodoItem;
import org.springframework.data.jpa.repository.JpaRepository;

public interface TodoRepository extends JpaRepository<TodoItem, Long> {
}
```

#### 4. Service
```java
package com.hftamayo.java.todo.service;

import com.hftamayo.java.todo.domain.TodoItem;
import com.hftamayo.java.todo.repository.TodoRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class TodoService {
    private final TodoRepository todoRepository;

    @Autowired
    public TodoService(TodoRepository todoRepository) {
        this.todoRepository = todoRepository;
    }

    public List<TodoItem> findAll() {
        return todoRepository.findAll();
    }

    public TodoItem save(TodoItem todoItem) {
        return todoRepository.save(todoItem);
    }
}
```

#### 5. Factory
```java
package com.hftamayo.java.todo.factory;

import com.hftamayo.java.todo.domain.TodoItem;

public class TodoFactory {
    public static TodoItem createTodoItem(String description) {
        TodoItem todoItem = new TodoItem();
        todoItem.setDescription(description);
        todoItem.setCompleted(false);
        return todoItem;
    }
}
```

#### 6. Bounded Context
Define bounded contexts by organizing your code into packages/modules that represent different subdomains.

By following these steps and concepts, you can align your project with Domain-Driven Development principles.

==API GATEWAY SCHEMA

![[api_gateway_schema.jpeg]]



==ejecutar un proyecto End to End:

![[ship_to_prod01.png]]![[ship_to_prod02.png]]