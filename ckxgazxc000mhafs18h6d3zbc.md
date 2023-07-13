---
title: "GraphQl API In Node.js With TypeGraphQl And Apollo"
seoTitle: "GraphQl API In Node.js With TypeGraphQl And Apollo"
seoDescription: "GraphQL is used to build APIs, It's like **REST**, But the reason why to use GraphQL instead of REST is that in graphql you can just give the data..."
datePublished: Tue Dec 21 2021 16:10:29 GMT+0000 (Coordinated Universal Time)
cuid: ckxgazxc000mhafs18h6d3zbc
slug: graphql-in-nodejs-with-apollo-typegraphql
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1650273535575/fWZxSXC89.png
tags: nodejs, typescript, graphql, apollo

---

# Why Use GraphQL

GraphQL is used to build API's, It's like **REST**, But the reason why to use GraphQL instead of REST is because in graphql you can just give the data and what user need is upto him/her

## Example

Let's just take a example of a simple REST vs GraphQL API

We have a REST endpoint that return array of posts from database, **_Not A Real Endpoint_**

```text
https://api.programmingfire.com/posts/
```

**Output**:

```json
[
  {
    "title": "post title 1",
    "description": "post description 1",
    "createdAt": 2021-09-13
  },
  {
    "title": "post title 2",
    "description": "post description 2",
    "createdAt": 2021-09-13
  },
  {
    "title": "post title 3",
    "description": "post description 3",
    "createdAt": 2021-09-13
  },
]
```

But let's just say we only want the title and description of the post but user can't do that
**But With GraphQL You Can Do It Let Me Show You**

**Input**:

```graphql
query {
  posts {
    title
  }
}
```

**Output**:

```json
{
  "data": {
    "posts": {
      "data": [
        {
          "title": "post title 1"
        },
        {
          "title": "post title 2"
        },
        {
          "title": "post title 3"
        }
      ]
    }
  }
}
```

As You Can See We Only Get Title Of Our Posts

## Setup GraphQL In Node.js With Apollo

Like any another Node.js app we need to run **init** command

```bash
# NPM
npm init

# YARN
yarn init
```

After initializing the app we will install some dependencies

#### Dependencies:

1. [Express](https://expressjs.com) - the server for node.js
2. [Apollo](https://www.apollographql.com/) - the server for graphql
3. [TypeGraphQL](https://typegraphql.com/) - for building schemas
4. [TypeScript](https://typescriptlang.org/) - the programming language

**dependencies**:

```bash
# NPM
npm install express apollo-server-express graphql apollo-server-core type-graphql

# YARN
yarn add express apollo-server-express graphql apollo-server-core type-graphql
```

**dev dependencies**:

```bash
# NPM
npm install --save-dev typescript @types/node @types/express nodemon

# YARN
yarn add -D typescript @types/node @types/express nodemon
```

Now let's just setup our **package.json** :

add a scripts section to your package.json :

```json
"scripts": {
    "start": "node dist/index.js",
    "dev": "nodemon dist/index.js",
    "watch": "tsc -w"
}
```

Now let's just create a **tsconfig.json** because we are going to use **typescript**

```json
{
  "compilerOptions": {
    "target": "es2017",
    "module": "commonjs",
    "lib": ["dom", "es6", "es2017", "esnext.asynciterable"],
    "skipLibCheck": true,
    "sourceMap": true,
    "outDir": "./dist",
    "moduleResolution": "node",
    "removeComments": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "strictFunctionTypes": true,
    "noImplicitThis": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "allowSyntheticDefaultImports": true,
    "esModuleInterop": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "resolveJsonModule": true,
    "baseUrl": "."
  },
  "exclude": ["node_modules"],
  "include": ["./src/**/*.ts"]
}
```

We are all set now we can start building the app

```ts
// src/index.ts

import express from "express";

const app = express();
const port = 3000;

app.get("/", (_, res) => {
  res.send("hello world");
});

app.listen(port);
```

to start the application run:

```bash
# NPM
npm run watch

# YARN
yarn watch
```

on the second terminal run:

```bash
# NPM
npm run dev

# YARN
yarn dev
```

open [http://localhost:3000/](http://localhost:3000/) and you should see **hello world**

Now let's just setup GraphQL with Apollo

```ts
// src/index.ts

const apolloServer = new ApolloServer({
  schema: await buildSchema({
    resolvers: [],
    validate: false,
  }),
  context: ({ req, res }): MyContext => ({ req, res, redis }),
  plugins: [ApolloServerPluginLandingPageGraphQLPlayground],
});

await apolloServer.start();

apolloServer.applyMiddleware({
  app,
  cors: false,
});
```

the complete **src/index.ts** should look like:

```ts
import express from "express";
import { ApolloServer } from "apollo-server-express";
import { buildSchema } from "type-graphql";
import { ApolloServerPluginLandingPageGraphQLPlayground } from "apollo-server-core";

const app = express();
const port = 3000;

const apolloServer = new ApolloServer({
  schema: await buildSchema({
    resolvers: [],
    validate: false,
  }),
  context: ({ req, res }) => ({ req, res }),
  plugins: [ApolloServerPluginLandingPageGraphQLPlayground],
});

await apolloServer.start();

apolloServer.applyMiddleware({
  app,
});

app.listen(port, () => console.log("server has started on localhost" + port));
```

now in your browser if you open [http://localhost:3000/graphql](http://localhost:3000/graphql) you should see [GraphQl Playground](https://www.graphqlbin.com/v2/new)

### What is GraphQL Playground

GraphQL Playground is a Graphical Tool To run graphql queries and mutation you can consider it as Postman for graphql

### Create Resolvers For GraphQL

now we are going to create a hello resolver for graphql using typegraphql

```ts
// src/resolvers/hello.ts

import { Resolver, Query } from "type-graphql";

@Resolver()
export class HelloResolver {
  @Query(() => String)
  hello(): string {
    return "hello world from graphql";
  }
}
```

now to use this resolver in our index.ts where we are defining our build schema like this

```ts
import { HelloResolver } from "./resolvers/hello";
const apolloServer = new ApolloServer({
  schema: await buildSchema({
    resolvers: [HelloResolver],
    validate: false,
  }),
  context: ({ req, res }) => ({ req, res }),
  plugins: [ApolloServerPluginLandingPageGraphQLPlayground],
});
```

now open graphql playground and type this query in

```graphql
query {
  hello
}
```

**output** :

```json
{
  "data": {
    "hello": "hello world from graphql"
  }
}
```