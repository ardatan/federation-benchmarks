<!-- README.md is generated from README.ecr, do not edit -->

# Apollo Federation Server benchmarks

This shows the benchmarks for different servers.

### Results

| Name                          | Language      | Server          | Latency avg      | Requests      |
| ----------------------------  | ------------- | --------------- | ---------------- | ------------- |
| [GraphQL Yoga with Response Cache](https://github.com/dotansimha/graphql-yoga) | Node.js | http | 46.54ms | 2.2kps |
| [GraphQL Yoga with JIT](https://github.com/dotansimha/graphql-yoga) | Node.js | http | 764.83ms | 120ps |
| [GraphQL Yoga](https://github.com/dotansimha/graphql-yoga) | Node.js | http | 916.90ms | 100ps |
| [Apollo Server](https://github.com/apollographql/apollo-server) | Node.js | Express | 1,234.12ms | 64ps |

### What it does?

For each server a gateway is implemented with the following sources;

- [Accounts](./common/accounts.js)
- [Inventory](./common/inventory.js)
- [Products](./common/products.js)
- [Reviews](./common/reviews.js)

On each HTTP request, the gateway receives the following complex query;

```graphql
fragment User on User {
  id
  username
  name
}
fragment Review on Review {
  id
  body
}
fragment Product on Product {
  inStock
  name
  price
  shippingEstimate
  upc
  weight
}
query TestQuery {
  users {
    ...User
    reviews {
      ...Review
      product {
        ...Product
      }
    }
  }
  topProducts {
    ...Product
    reviews {
      ...Review
      author {
        ...User
      }
    }
  }
}
```

The API is served over HTTP using a common web server and load tested using [bombardier](https://github.com/codesenberg/bombardier).