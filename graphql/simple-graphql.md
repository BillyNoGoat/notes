### GraphQL

GraphQL is a query language for API's and a server-side runtime for executing queries by using a type system you define for your data. It is a backend for your existing code and data.

A GraphQL service is created by defining types and fields on those types. An example is a GraphQL service which tells us who the logged in user is as well as the user's name which might look like the following:

```
type Query {
  me: User
}

type User {
  id: ID
  name: String
}
```

Along with the functions for each field on each type:

```
function Query_me(request) {
  return request.auth.user;
}

function User_name(user) {
  return user.getName();
}
```

Once a GraphQL service is running (typically at a URL on a web service) it can be sent GraphQL queries to validate and execute. A received query is first checked to ensure it only refers to the types and fields defined, then runs the provided functions to produce a result.

For example the query:
```
{
  "me": {
    "name": "Luke Skywalker"
  }
}
```

### Queries and mutations
#### fields
Simply., GraphQL is about asking for specific fields on objects. Here is a very simple query and the result we get when we run it:

Query:

```
{
  hero {
    name
  }
}
```
Result:
```
{
  "data": {
    "hero": {
      "name": "R2-D2"
    }
  }
}
```

We can see that the query has the same shape as the result. This is essential to GraphQL as you always get back what you expect and the server knows what fields the client is asking for.

The field `name` returns a `String` type, in this case the name of the main hero of Star Wars "R2-D2"

In this example, it returned us a string when we asked for the name but this can also refer to Objects. In this case, we can make a sub-selection of fields for that object. GraphQL queries can traverse related objects and their fields so clients can fetch lots of related data in a single request, instead of making several roundtrips as one would need in a REST api.


Query:

```
{
  hero {
    name
    # Queries can have comments!
    friends {
      name
    }
  }
}
```

Result:

```
{
  "data": {
    "hero": {
      "name": "R2-D2",
      "friends": [
        {
          "name": "Luke Skywalker"
        },
        {
          "name": "Han Solo"
        },
        {
          "name": "Leia Organa"
        }
      ]
    }
  }
}
```

From the above example observe that we can make comments in our query but more importantly the `friends` field returns an array of items. GraphQL queries look the same for both single items or lists of items, however we know which one to expect based on what's indicated in the schema.

#### Arguments
If the only thing we could do was traverse objects and their fields, GraphQL would be already a very useful language for data fetching. But when you add the ability to pass arguments to fields, we have more at our disposal.

Request:
```
{
  human(id: "1000") {
    name
    height
  }
}
```

Response:
```
{
  "data": {
    "human": {
      "name": "Luke Skywalker",
      "height": 1.72
    }
  }
}
```

In a system like REST, you can only pass a single set of arguments - the query parameters and URL segments in your request. But in GraphQL, every field and nested object can get it's own set of arguments making GraphQL a complete replacement for making multiple API fetches. You can even pass arguments into scalar fields, to implement data transformations once on the server, instead of on every client separately:

Request:
```
{
  human(id: "1000") {
    name
    height(unit: FOOT)
  }
}
```

Response:

```
{
  "data": {
    "human": {
      "name": "Luke Skywalker",
      "height": 5.6430448
    }
  }
}
```

Arguments can be of many different types. In the above example, we have used an Enumeration type, which represents one of a finite set of options (in this case, units of length, either METER or FOOT). GraphQL comes with a default set of types, but a GraphQL server can also declare it's own custom types, as long as they can be serialised into your transport format.
