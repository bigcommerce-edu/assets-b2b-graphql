# Super Admins

## Logging In as a Super Admin

**Login example:**

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
        result {
            token
        }
    }
}
```

## Fetching the Super Admin's Companies

**Get companies example:**

```
query GetSuperAdminCompanies(
    $superAdminId: Int!
) {
    superAdminCompanies(
        superAdminId: $superAdminId
    ) {
        pageInfo {
            hasNextPage
            hasPreviousPage
            startCursor
            endCursor
        }
        totalCount
        edges {
            node {
                companyId
                companyName
                companyAdminName
                companyEmail
            }
        }
    }
}
```

**Example response:**

```json
{
    "data": {
        "superAdminCompanies": {
            "pageInfo": {
                "hasNextPage": false,
                "hasPreviousPage": false,
                "startCursor": "YXJy...",
                "endCursor": "YXJy..."
            },
            "totalCount": 5,
            "edges": [
                {
                    "node": {
                        "companyId": 1112233,
                        "companyName": "John's Widgets",
                        "companyAdminName": "John Doe",
                        "companyEmail": "johndoe@example.com"
                    }
                },
                ...
            ]
        }
    }
}
```

## Masquerading

**Begin masquerade example:**

```
mutation BeginMasquerade(
    $companyId: Int!,
    $userId: Int!
) {
    superAdminBeginMasquerade(
        companyId: $companyId,
        userId: $userId
    ) {
        userInfo {
            email
            firstName
            lastName
            phoneNumber
        }
    }
}
```

**Example response:**

```json
{
    "data": {
        "superAdminBeginMasquerade": {
            "userInfo": {
                "email": "superadmin@example.com",
                "firstName": "Joe",
                "lastName": "Super",
                "phoneNumber": "111-222-3333"
            }
        }
    }
}
```

**Get masquerade company example:**

```
query GetMasqueradeInfo(
    $customerId: Int!
) {
    superAdminMasquerading(
        customerId: $customerId
    ) {
        id
        companyName
        addressLine1
        addressLine2
        city
        state
        zipCode
        country
    }
}
```

**Example response:**

```json
{
    "data": {
        "superAdminMasquerading": {
            "id": "1112233",
            "companyName": "John's Widgets",
            "addressLine1": "123 Park Central",
            "addressLine2": null,
            "city": "Austin",
            "state": "Texas",
            "zipCode": "78726",
            "country": "United States"
        }
    }
}
```

**End masquerade example:**

```
mutation EndMasquerade(
    $companyId: Int!,
    $userId: Int!
) {
    superAdminEndMasquerade(
        companyId: $companyId,
        userId: $userId
    ) {
        message
    }
}
```

**Example response:**

```json
{
    "data": {
        "superAdminEndMasquerade": {
            "message": "Success"
        }
    }
}
```

## GraphQL Requests While Masquerading

**Get orders example:**

```
query GetOrders {
    allOrders {
        pageInfo {
            hasNextPage
            hasPreviousPage
            startCursor
            endCursor
        }
        totalCount
        edges {
            node {
                id
                bcOrderId
                createdAt
                userId
                bcCustomerId
                ...
            }
        }
    }
}
```
