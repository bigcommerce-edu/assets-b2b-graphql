# Common Conventions

## GraphQL Variables

**Example without arguments:**

```
mutation {
    login(
        loginData: {
            storeHash: {Store Hash},
            email: {Email address},
            password: {Password}
        }
    ) {
        ...
    }
}
```

**Example with arguments:**

```
mutation LogIn(
    $storeHash: String!,
    $email: String!,
    $password: String!
) {
    login(
        loginData: {
            storeHash: $storeHash,
            email: $email,
            password: $password
        }
    ) {
        ...
    }
}
```

**Variables input:**

```json
{
    "storeHash": "pki...",
    "email": "user@example.com",
    "password": "MyPassword123..."
}
```

## Cursor Lists and Pagination

**Example request:**

```
query GetUsers(
    $companyId: Int!
) {
    users(
        companyId: $companyId,
        first: 10
    ) {
        pageInfo {
            hasNextPage
            hasPreviousPage
            startCursor
            endCursor
        }
        totalCount
        edges {
            cursor
            node {
                id
            }
        }
    }
}
```

**Forward pagination example:**

```
query GetUsers(
    $companyId: Int!,
    $after: String
) {
    users(
        companyId: $companyId,
        after: $after,
        first: 10
    ) {
        pageInfo {
            hasNextPage
            hasPreviousPage
            startCursor
            endCursor
        }
        totalCount
        edges {
            cursor
            node {
                id
            }
        }
    }
}
```

**Pagination variables:**

```json
{
    "companyId": 1112233,
    "after": "abc123"
}
```

**Backward pagination example:**

```
query GetUsers(
    $companyId: Int!,
    $before: String
) {
    users(
        companyId: $companyId,
        before: $before,
        last: 10
    ) {
        pageInfo {
            hasNextPage
            hasPreviousPage
            startCursor
            endCursor
        }
        totalCount
        edges {
            cursor
            node {
                id
            }
        }
    }
}
```

**Offset example:**

```
query GetUsers(
    $companyId: Int!,
    $offset: Int
) {
    users(
        companyId: $companyId,
        offset: $offset,
        first: 10
    ) {
        ...
    }
}
```

## Store Hash and Channel ID

**Querying channel ID:**

```
query {
    quotes {
        edges {
            node {
                id
                channelId
            }
        }
    }
}
```

## Extra Data Fields

**Custom fields example:**

```
query GetInvoices {
    invoices {
        edges {
            node {
                extraFields {
                    fieldName
                    fieldValue
                }
            }
        }
    }
}
```

**Fixed extra fields example:**

```
query {
    allOrders {
        edges {
            node {
                extraInt1
                extraInt2
                extraInt3
                extraInt4
                extraInt5
                extraStr1
                extraStr2
                extraStr3
                extraStr4
                extraStr5
            }
        }
    }
}
```

[Next](../../Module02/01_CompaniesAndAddresses/01_CompaniesAndAddresses.md)
