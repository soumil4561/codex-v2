GraphQL is a query language for your API, and a server-side runtime for executing queries using a type system you define for your data. *GraphQL isn’t tied to any specific database or storage engine—it is backed by your existing code and data.*

## Schemas and Types
The GraphQL [type system](https://spec.graphql.org/draft/#sec-Type-System) describes what data can be queried from the API. The collection of those capabilities is referred to as the service’s _schema_ and clients can use that schema to send queries to the API that return predictable results.

*At the core of GraphQL are its **type system** and **schema**, which define the structure of your API and what data clients can request.*

### Type Definitions
#### ScalarTypeDefinition
Defines a **primitive value type** that cannot be broken down further. GraphQL built ins: 
- `Int` signed 32‐bit integer
- `Float` A signed double-precision float
- `String` A UTF‐8 character sequence
- `Boolean` true or false
- `ID` A unique identifier (serialized as a string)

#### ObjectTypeDefinition
Defines a **type with fields**, like a class or record. Example:
```graphql
type User {
  id: ID!
  name: String!
  email: String
}
```

Each field may return a scalar, another object, a list, or even a custom type.

### InterfaceTypeDefinition
Defines a **contract** that other types must implement.
```graphql
interface Node {
  id: ID!
}

type User implements Node {
  id: ID!
  name: String!
}
```

Interfaces are useful when you want to return a generic type from a query and ensure all results share some common fields.

### UnionTypeDefinition
Allows a field to return **one of several types** (but not all have to share fields).
```graphql
union SearchResult = User | Post | Comment
```

### EnumTypeDefinition
Define a set of constant values

```graphql
enum Role {
  ADMIN
  USER
  GUEST
}
```

Enums are useful for fields with restricted values, like status, role, or mode.

### InputTypeDefinition
Defines a structured input used in mutations or queries. 

```graphql
input CreateUserInput {
  name: String!
  email: String!
  age: Int
}
```


## Directives
In certain instances where field arguments are insufficient or certain common behaviours must be replicated in multiple locations, [directives](https://spec.graphql.org/draft/#sec-Type-System.Directives) allow us to modify parts of a GraphQL schema or operation by using an `@` character followed by the directive’s name.

**Directives** are special annotations in GraphQL, prefixed with `@`, that instruct the GraphQL engine to handle parts of a query or schema in a specific way.

There are two main types:
1. **Query Directives** – Used in operations (query/mutation/subscription)
2. **Schema Directives** – Used when defining the schema

#### Query Built-ins
1. @include(if: Boolean): Includes a field **only if the condition is true**.
```graphql
query getUser($withEmail: Boolean!) {
  user(id: "123") {
    name
    email @include(if: $withEmail)
  }
}
```

2. @skip(if:Boolean):Skips a field if the condition is true (inverse of `@include`).
```graphql
query getUser($skipEmail: Boolean!) {
  user(id: "123") {
    name
    email @skip(if: $skipEmail)
  }
}
```

#### Schema/Custom Directives
You can **define your own directives** in the schema to control how fields or types behave.
```graphql
#define the directive
directive @deprecated(reason: String = "No longer supported") on FIELD_DEFINITION | ENUM_VALUE

#use in schema
type User { fullName: String name: String @deprecated(reason: "Use `fullName`.")}
```

*Note: While you won’t need to define the `@deprecated` directive in your schema explicitly if you’re using a GraphQL implementation that supports SDL, it’s underlying definition would look like the one defined*

