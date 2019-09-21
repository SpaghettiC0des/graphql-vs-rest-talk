
## WTF is GraphQL?
It is an open-source query language for API's. And before you ask, no, it is not a database query language, like SQL. It was developed by `Facebook` on 2012 and it was release to the public on 2015.

It is not tied to any specific programming language(s), it is more like a specification than an implementation.

## GraphQL & REST diagram

![toptal-blog-image-1512397485791-81c8cdf5eeaf7bb1ea81fd949136c081.png](https://uploads.toptal.io/blog/image/124855/toptal-blog-image-1512397485791-81c8cdf5eeaf7bb1ea81fd949136c081.png)

## Advantages
1. It gives you the power to shape the response the way you want it.
1. No more overfetching, you get exactly what you request. 
2. Single request, instead of multiple request.
3. More strict, because of the schema first development.
4. Better developer experience, easier to maintain.
5. Better tooling
6. Automatic API documentation
7. Less communication overhead between the Mobile and Web team.
8. Large community
9. Adopted by big companies like Facebook, PayPal, Github, Pinterest, Shopify, Atlassian, Twitter and many more. Head over to graphql.org/users to know more.

## Disadvantages
1. Since this is a relatively new technology, the learning curve is steep.
2. May feel awkward at first.


# Security
*_With great power, come with great RESPONSE ability._*

GraphQL solves the problem of API request efficiency, but just like other frameworks or patterns that solve a specific problem, it will introduce another problem. It is up to the developer to prevent this security risks.

As I've said earlier, GraphQL clients can craft the response shape, complexity or depth the way they wanted. Our servers should know how to handle this situations.

### Here the suggested solutions for securing a GraphQL API

## Timeout
The easiest solution is to implement a query timeout to prevent evil clients or even the legitemate client to execute complex queries.


## Maximum query depth
Abusive query example
```graphql             
query iAMhAckErsx {    // Depth 0
  user(id: 123) {      // Depth 1
    posts {            // Depth 2
      author {         // Depth 3
        posts {        // Depth 4
          author {     // Depth 5
            posts {    // Depth 6
              author { // Depth 7
                posts  // Depth 8
              }
            }
          }
        }
      }
    }
  }
}
```
 
For example we've configured the server(laravel-lighthouse) to have a depth of 4, our previous query example would fail and the server will spit out an error, something like this.
```json
{
  "errors": [
    {
      "message": "Query has depth of 8, which exceeds max depth of 4"
    }
  ]
}
```

## Query Complexity
A common strategy is to give the query a complexity of `1`, so in this example, we have a total of `3` complexity. If we have set the query complexity to `2`, this would fail.
```
query {
  author(id: "abc") { # complexity: 1
    posts {           # complexity: 1
      title           # complexity: 1
    }
  }
}
```
We can configure the complexity per field or the parameter.

## Throttling
Leaky bucket algorithm.
If we have a maximum server time of `1000ms`, and queries takes an average of `200ms` to finish, means a client calling this operation 5 times in a second will be blocked and should wait for the server time to reset.
