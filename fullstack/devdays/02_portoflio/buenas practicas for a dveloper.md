
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

