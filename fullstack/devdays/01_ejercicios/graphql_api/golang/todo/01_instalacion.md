nuestGiven the [`Todo`](command:_github.copilot.openSymbolFromReferences?%5B%7B%22%24mid%22%3A1%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fgolang%2Fmyprojects%2Fgographqltodo%2Fapi%2Fv1%2Fmodels%2Ftodo.go%22%2C%22scheme%22%3A%22file%22%7D%2C%7B%22line%22%3A12%2C%22character%22%3A5%7D%5D "api/v1/models/todo.go") model you've provided, let's continue with the scenario of migrating your API to GraphQL, focusing on integrating this model into a GraphQL setup using `gqlgen` for Go.

### Step 1: Define GraphQL Schema for Todo

First, you need to define a GraphQL schema that represents your [`Todo`](command:_github.copilot.openSymbolFromReferences?%5B%7B%22%24mid%22%3A1%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fgolang%2Fmyprojects%2Fgographqltodo%2Fapi%2Fv1%2Fmodels%2Ftodo.go%22%2C%22scheme%22%3A%22file%22%7D%2C%7B%22line%22%3A12%2C%22character%22%3A5%7D%5D "api/v1/models/todo.go") model. Create a file named `schema.graphql` in your project's root or under a `/graph` directory, and define your schema:

```graphql
type Todo {
  id: ID!
  title: String!
  done: Boolean!
  body: String!
}

type Query {
  todos: [Todo!]!
  todo(id: ID!): Todo
}

type Mutation {
  createTodo(title: String!, body: String!): Todo!
  updateTodo(id: ID!, title: String, done: Boolean, body: String): Todo
  deleteTodo(id: ID!): Boolean
}
```

### Step 2: Generate Code with gqlgen

Assuming you've already initialized `gqlgen` in your project, the next step is to generate the Go code based on your schema. If you haven't customized `gqlgen.yml`, do so to match your project structure, then run:

```sh
go get github.com/99designs/gqlgen
gqlgen init
go run github.com/99designs/gqlgen generate
```

si no me funcionó el anterior go get puedo probar con go install siempre desde el root directory del proyecto:
``` alternativo
go install github.com/99designs/gqlgen@latest
gqlgen version

```

![[Pasted image 20240708151328.png]]

![[Pasted image 20240708151520.png]]

==notese que estoy usando referencias absolutas y es porque modifiqué el GOPATH de todo mi sistema, los pasos son así:

1. verificar si gqlgen esta instalado en mi GOPATH

![[Pasted image 20240708151819.png]]

2.  agregar GOPATH en el PATH del sistema:


![[Pasted image 20240708151942.png]]

al final del bashrc veo el cambio:

![[Pasted image 20240708152108.png]]


### Step 3: Implement Resolvers

`gqlgen` generates resolver stubs in `graph/schema.resolvers.go`. You need to implement the logic for querying and mutating data. Here's an example of how you might implement the `todos` query:

```go
func (r *queryResolver) Todos(ctx context.Context) ([]*models.Todo, error) {
	var todos []*models.Todo
	result := r.DB.Find(&todos)
	return todos, result.Error
}
```

### Step 4: Set Up GraphQL Handler in Gin

Integrate the GraphQL server with Gin by setting up a handler:

```go
package main

import (
	"github.com/99designs/gqlgen/graphql/handler"
	"github.com/99designs/gqlgen/graphql/playground"
	"github.com/gin-gonic/gin"
	"your_project/graph"
	"your_project/graph/generated"
)

func graphqlHandler() gin.HandlerFunc {
	h := handler.NewDefaultServer(generated.NewExecutableSchema(generated.Config{Resolvers: &graph.Resolver{}}))
	return func(c *gin.Context) {
		h.ServeHTTP(c.Writer, c.Request)
	}
}

func playgroundHandler() gin.HandlerFunc {
	h := playground.Handler("GraphQL", "/query")
	return func(c *gin.Context) {
		h.ServeHTTP(c.Writer, c.Request)
	}
}

func main() {
	r := gin.Default()
	r.POST("/query", graphqlHandler())
	r.GET("/", playgroundHandler())
	r.Run()
}
```

### Step 5: Test Your GraphQL API

After setting up your server, test your API using GraphQL playgrounds or tools like Postman. Make sure to test all queries and mutations to ensure they work as expected.

### Step 6: Refactor and Optimize

As you test, you might find areas for optimization, such as adding indexes to your database or refining your GraphQL schema. Take the time to refine and optimize your implementation.

This scenario provides a basic outline for migrating a REST API using Gin and GORM to a GraphQL API using `gqlgen`. Each step can be expanded based on the specific needs of your project, such as adding authentication, pagination, or subscription support.