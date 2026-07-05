# Memberful GraphQL API

Memberful (a Patreon-owned membership and subscription platform) exposes a **native GraphQL API** - this is a real, documented GraphQL surface, not a REST-to-GraphQL projection.

**Endpoint:** `POST https://ACCOUNT.memberful.com/api/graphql` (per-account; replace `ACCOUNT` with your Memberful subdomain)
**Authentication:** API key as a bearer token (`Authorization: Bearer <api-key>`) or an OAuth 2.0 access token. API keys are created under Settings → Custom applications.
**Request shape:** POST with a `query` parameter containing the GraphQL query or mutation.
**Errors:** returned as HTTP 200 with an `errors` array in the JSON body.
**Pagination:** cursor-based, using `first` / `after` / `last` / `before`.
**Documentation:** the authoritative, always-current schema is the live **GraphQL API Explorer** inside the Memberful dashboard.

- Overview: https://memberful.com/help/custom-development-and-api/memberful-api/
- Reference: https://memberful.com/docs/api-reference/memberful-api
- Schema (modeled): [memberful-schema.graphql](memberful-schema.graphql)

## Terminology note

In the Memberful **dashboard**, what a member subscribes to is called a "Plan"; in the **API** that object is a `Pass`, and `Plan` refers to a pricing option (for example "$10 / month") within a pass. This file and the schema follow the API terminology.

## Example query — a member with subscriptions

```graphql
query {
  member(id: "1") {
    id
    fullName
    email
    subscriptions {
      id
      active
      plan {
        id
        label
      }
    }
  }
}
```

## Example mutation — create a member

```graphql
mutation {
  memberCreate(
    email: "new.member@example.com"
    fullName: "New Member"
  ) {
    id
    email
    fullName
  }
}
```

## Example query — paginated orders

```graphql
query {
  orders(first: 25, after: "eyJvIjoxMH0") {
    edges {
      cursor
      node {
        id
        number
        status
        totalCents
        member { email }
      }
    }
    pageInfo {
      hasNextPage
      endCursor
    }
  }
}
```

> The confirmed objects and sample operations above (`member`, `members`, `Subscription`, `Pass`, `Plan`, `memberCreate`, cursor pagination) come directly from Memberful's public documentation. Additional fields in the schema file are honestly modeled from the documented object descriptions and should be verified against the live GraphQL API Explorer, which is Memberful's source of truth.
