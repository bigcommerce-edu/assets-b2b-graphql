# Authentication

## Logging in with the BigCommerce GraphQL Storefront API

**Example login request:**

```
mutation Login(
    $email: String!,
    $password: String!
) {
    login(
        email: $email,
        password: $password
    ) {
        result
        customer {
            entityId
            email
            firstName
            lastName
        }
        customerAccessToken {
            value
            expiresAt
        }
    }
}
```

**Example response:**

```json
{
    "data": {
        "login": {
            "result": "success",
            "customer": {
                "entityId": 123,
                "email": "user@example.com",
                "firstName": "Jane",
                "lastName": "Doe"
            },
            "customerAccessToken": {
                "value": "abc123...",
                "expiresAt": "2024-12-31T00:00:00Z"
            }
        }
    }
}
```

**Example request with customer access token (server-side):**

```
POST https://www.example.com/graphql
Content-Type: "application/json"
Authorization: "Bearer {storefront_token}"
X-Bc-Customer-Access-Token: "{customer_access_token}"

{
    "query": "query { customer { entityId email } }"
}
```

### Syncing with the B2B GraphQL API

**Example request to exchange a customer access token for a B2B token:**

```
POST https://api-b2b.bigcommerce.com/api/io/auth/customers/storefront

{
  "customerId": 999,
  "channelId": 1,
  "customerAccessToken": {
    "value": "<token value>",
    "expires_at": "2024-12-31T00:00:00.0Z"
  }
}
```

**Example response:**

```json
{
  "code": 200,
  "data": {
    "token": "eyJ0eX..."
  },
  "meta": {
    "message": "success"
  }
}
```

## Logging in with the B2B GraphQL API

**Example login request:**

```
mutation {
    login(
        loginData: {
            storeHash: "pki...",
            email: "user@example.com",
            password: "MyPassword123..."
        }
    ) {
        result {
            token
            user {
                id
                bcId
                firstName
                lastName
                email
            }
        }
    }
}
```

**Example response:**

```json
{
    "data": {
        "login": {
            "result": {
                "token": "eyJ0eX...",
                "user": {
                    "id": "8889911",
                    "bcId": 28,
                    "firstName": "B2B",
                    "lastName": "User",
                    "email": "user@example.com"
                }
            }
        }
    }
}
```

**Example request with token:**

```
POST https://api-b2b.bigcommerce.com/graphql
Accept: "application/json"
Content-Type: "application/json"
Authorization: "Bearer eyJ0eX..."

{
    "query": "..."
}
```

## Syncing Logins in Stencil

**Example request for Customer Login API token:**

```
mutation {
    login(
        loginData: {
            ...
        }
    ) {
        result {
            storefrontLoginToken
            token
            user {
                ...
            }
        }
    }
}
```

**Example request with Current Customer API token:**

```
mutation {
    authorization(
        authData: {
            bcToken: {JWT value}
        }
    ) {
        result {
            token
        }
    }
}
```

## Syncing Logins with BigCommerce GraphQL

The `storefrontToken` mutation goes the other direction: use it with a B2B token to obtain a BigCommerce GraphQL Storefront API token. Client-side use only; the resulting token is not suitable for server-side requests.

**Example request:**

```
mutation {
    storeFrontToken(
        storeFrontTokenData: {
            storeHash: "pki...",
            channelId: 1,
            expiresAt: 2147483647,
            allowedCorsOrigins: ["https://example.com"]
        }
    ) {
        token
    }
}
```

## Obtaining the Token from the Buyer Portal

**Example call:**

```javascript
window.b2b.utils.user.getB2BToken()
```
