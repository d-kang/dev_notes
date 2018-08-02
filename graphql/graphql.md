[Node.js Tutorial - Introduction](https://www.howtographql.com/graphql-js/0-introduction/)

[Hackernews Project](https://github.com/howtographql/graphql-js)


- Dependencies
  - graphql-yoga
  - prisma cli
  - prisma-binding
  - graphql-cli

`yarn add graphql-yoga`
`npm install -g prisma`
`yarn add prisma-binding`
`npm install -g graphql-cli`

## GraphQL-yoga

**graphql-yoga** is a fully-featured GraphQL server. It is based on Express.js and a few other libraries to help you build production-ready GraphQL servers.

- GraphQL spec-compliant
- Supports file upload
- Realtime functionality with GraphQL - subscriptions
- Works with TypeScript typings
- Out-of-the-box support for GraphQL Playground
- Extensible via Express middlewares
- Resolves custom directives in your GraphQL - schema
- Query performance tracing
- Accepts both application/json and - application/graphql content-types
- Runs everywhere: Can be deployed via now, up,- AWS Lambda, Heroku etc.


## GraphQL Playground
What you’ll then see is a GraphQL Playground, a powerful “GraphQL IDE” that lets you explore the capabilities of your API in an interactive manner.





## GraphQL
- One of the core benefits of GraphQL in general: It enforces that the API actually behaves in the way that is promised by the schema definition! This way, everyone who has access to the GraphQL schema can always be 100% sure about the API operations and data structures that are returned by the API.
- Every GraphQL schema has three special root types, these are called Query, Mutation and Subscription. The root types correspond to the three operation types offered by GraphQL: queries, mutations and subscriptions. The fields on these root types are called root field and define the available API operations.
-


## In depth article on GraphQL Schemas
[GraphQL Server Basics: GraphQL Schemas, TypeDefs & Resolvers Explained](https://www.prisma.io/blog/graphql-server-basics-the-schema-ac5e2950214e/)

## GraphQL Schema Definition Guide
[GraphQL SDL — Schema Definition Language](https://www.prisma.io/blog/graphql-sdl-schema-definition-language-6755bcb9ce51/)


two(!)