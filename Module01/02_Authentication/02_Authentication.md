# Authentication

## The Authentication Process

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

## Obtaining the Token from the Buyer Portal

**Example call:**

```javascript
window.b2b.utils.user.getB2BToken()
```

[Next](../03_CommonConventions/03_CommonConventions.md)
