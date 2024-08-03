
==proyectos para portafolio midu:

![[proyectos 2024 portfolio.jpeg]]

==road to dev senior:

![[road to dev senior.png]]

![[solid_principles.gif]]

![[sdlc_2023.gif]]

![[code review pyramid.jpeg]]

![[E2E software lifecycle.gif]]

12 Design Patterns
![[design_patterns.gif]]

![[improveapiperformance.gif]]

## ==REQUERIMIENTOS PARA QUE UNA APLICACION SEA PRODUCTION READY (performance, realibility at scale):

- Caching:
- Data load strategies: using DataLoader to batch requests and reduce data load
- Efficient error handling

## ==COMBINACION DE TECNOLOGIAS PARA UN BACKEND ESCRITO EN GOLANG:


| Task                          | Responsable      | Funcion                                                                                                                                      |
| ----------------------------- | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| Routing                       | GraphQL (gqlgen) | de manera inherente los servers GraphQL gestionan el ruteo de queries provenientes queries y mutations hacia el resolver apropiado           |
| Logging, CORS, authentication | Gin              | Anque estas funciones tambien puede ser desempeñadas por GraphQL, si ya se cuenta con middlewares o si se esta familiarizado puede retomarse |
| Database operations           | Gorm             | migrations, hooks, CRUD ops, los modelos y otros archivos deben ser ajustados para ser compatibles con GraphQL                               |

## ==MOTIVOS PARA USAR UNA INTERFACE EN LUGAR DE UNA CLASE QUE CONTIENE LA BUSINESS LOGIC

**The primary reason for using an interface like `UserInfoProvider` instead of directly injecting the concrete implementation `UserDetailServiceBasedUserInfoProvider` is to promote loose coupling and improve code maintainability.**

### Advantages of using an interface:

1. **Dependency Inversion:**
    - By depending on an abstraction (the interface) rather than a concrete implementation, you adhere to the Dependency Inversion Principle. This makes your code more flexible and testable.
2. **Testability:**
    - You can easily mock or stub the `UserInfoProvider` interface for unit testing, isolating the `AuthenticationFilter` from the underlying implementation details.
3. **Extensibility:**
    - If you need to change the way user information is retrieved (e.g., using a different data source), you can create a new implementation of `UserInfoProvider` without affecting the `AuthenticationFilter`.

**In summary,** using an interface creates a more maintainable and flexible architecture, allowing you to easily adapt to changes in the future without modifying the `AuthenticationFilter` class.

By adhering to these principles, you'll enhance the overall quality and robustness of your Spring Boot application.

## ==SOLVING A CIRCULAR DEP IN JSB:

The error message you provided indicates an issue with circular dependencies in your Spring Boot application. Here's a breakdown of the error and some steps you can take to fix it:

**Error Breakdown:**

The message starts with `org.springframework.context.ApplicationContextException: Unable to start web server`. This tells you that Spring encountered an error while trying to start your web server (likely Tomcat in this case).

Digging deeper, you see `org.springframework.beans.factory.UnsatisfiedDependencyException`. This means Spring couldn't find the required dependencies for creating some beans in your application.

Following the chain of exceptions, you see a loop:

- `userDetailsService` depends on `springSecurityConfig`
- `springSecurityConfig` depends on `userServiceImpl`
- `userServiceImpl` depends on `springSecurityConfig`

This creates a circular reference, where each bean needs the other to be created first. Spring gets stuck in this loop and can't create any of them.

**Resolving Circular Dependencies:**

There are a few ways to fix this problem:

1. **Refactor your code:** Analyze the dependencies between your beans and see if you can restructure your code to break the circular reference. Perhaps a bean can be redesigned to not depend on another one directly.
2. **Use Lazy Initialization:** Annotate a bean with `@Lazy` to defer its initialization until it's actually needed. This can help break circular dependencies in some cases.
3. **Introduce an intermediary bean:** Create a new bean that acts as a bridge between the two conflicting beans. This intermediary bean can have the necessary dependencies and bridge the gap between the originally circular ones.

**Additional Tips:**

- Review your Spring configuration files and ensure you haven't accidentally introduced these dependencies.
- Use a debugger to step through your application initialization and see where the circular dependency is happening.

By following these steps and analyzing your specific code, you should be able to identify and address the circular dependency issue and get your Spring Boot application running smoothly.

## Understanding the Circular Dependency

Before we dive into potential solutions, it's essential to clarify the exact nature of the circular dependency.

**Based on the provided error messages, it seems like a chain of dependencies exists:**

- `AuthenticationFilter` depends on `UserInfoProvider`.
- `UserInfoProvider` likely depends on `UserDetailsService` (either directly or indirectly).
- `UserDetailsService` likely depends on `UserRepository` or a similar data access layer.

If this is the case, introducing an intermediary bean might not be the most effective solution. Instead, refactoring the code to break the dependency chain would be more appropriate.

### Refactoring Approach

**1. Identify the Root Cause:**

- Carefully examine the `UserServiceImpl` class to understand how it retrieves user details.
- Determine if it's directly accessing the database or relying on the `SecurityContextHolder`.

**2. Break the Dependency:**

- If `UserServiceImpl` relies on the `SecurityContextHolder`, consider refactoring it to fetch user details directly from the database using a `UserRepository` or a similar data access layer.
- If `UserServiceImpl` already uses a `UserRepository`, ensure it's not indirectly relying on the `SecurityContextHolder`.

**3. Refactored `UserServiceImpl`:**

Java

```
@Service
public class UserServiceImpl implements UserService {

    private final UserRepository userRepository;    1.  github.com github.com

    public UserServiceImpl(UserRepository userRepository) {
        this.userRepository = userRepository;    1.  stackoverflow.com stackoverflow.com
    }

    public Optional<User> getUserByEmail(String email) {
        return userRepository.findByEmail(email);
    }

    // ... other    1.  github.com github.com methods
}
```

Use code [with caution.](/faq#coding)

**4. Adjust `UserDetailsService`:**

- Modify the `UserDetailsService` to fetch user details using the `UserServiceImpl`.

``

**Additional Considerations:**

- **Dependency Injection:** Ensure that all dependencies are correctly injected using constructor injection or `@Autowired`.
- **Lazy Initialization:** If refactoring is not feasible, consider using `@Lazy` on specific beans.
- **Spring Data JPA:** If you're using Spring Data JPA for your data access layer, leverage its features to simplify data retrieval.

By following these guidelines and refactoring your code, you should be able to resolve the circular dependency and create a more maintainable application.