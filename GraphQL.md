https://hasura.io/learn/graphql/android/optimistic-update-mutations/1-update-todos/


      query getNewPublicTodos($latestVisibleId: Int)
      {
          todos(where: {is_public: {_eq: true}, 
      		id:{_gt: $latestVisibleId }},
       order_by: { created_at : desc })
      {
         Id
         Title    created_at 
         User{ name }
      	}
      }



    query LaunchList($cursor:String) {
      launches(after: $cursor) {
        cursor
        launches {
          id
          site
          mission {
            name
            missionPatch(size: SMALL)
          }
        }
        hasMore
      }
    }



- Mutations are GraphQL operations that modify back-end data. As a convention in GraphQL, queries are read operations and mutations are write operations.

— Subscriptions are long-lived GraphQL read operations that can update their response over time, enabling clients to receive new data as it becomes available.

— Fragments are reusable units in GraphQL that allow developers to construct sets of fields that you can include in multiple operations. Fragments are helpful when constructing queries that require repetitive fields or complex data structures that need to be broken down into smaller chunks.
Apollo Kotlin supports both forms of GraphQL fragments:
* Named fragments: enable you to reuse a set of fields across multiple operations.
* Inline fragments: enable you to access fields of polymorphic types.

**Named Fragments:**

      fragment UserFields on User {
        name
        email
      }

      query {
        user(id: 1) {
          ...UserFields
        }
      }

**Inline Fragments:**

          query {
            user(id: 1) {
              ... on Admin {
                name
                email
              }
            }
          }
