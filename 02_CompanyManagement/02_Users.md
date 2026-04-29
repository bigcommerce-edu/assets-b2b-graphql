# Users

## Fetching a Company's Users

**Get users example:**

```
query GetUsers(
    $companyId: Int!
) {
    users(
        companyId: $companyId
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
                id
                bcId
                firstName
                lastName
                email
                phone
                role
                companyRoleId
                companyRoleName
            }
        }
    }
}
```

**Example response:**

```json
{
    "data": {
        "users": {
            "pageInfo": {
                "hasNextPage": true,
                "hasPreviousPage": false,
                "startCursor": "YXJy...",
                "endCursor": "YXJy..."
            },
            "totalCount": 11,
            "edges": [
                {
                    "node": {
                        "id": "99988877",
                        "bcId": 27,
                        "firstName": "John",
                        "lastName": "Doe",
                        "email": "johndoe@example.com",
                        "phone": "111-222-3333",
                        "role": 2,
                        "companyRoleId": 11223,
                        "companyRoleName": "Junior Buyer"
                    }
                },
                ...
            ]
        }
    }
}
```

**Get user example:**

```
query GetUser(
    $companyId: Int!,
    $userId: Int!
) {
    user(
        companyId: $companyId,
        userId: $userId
    ) {
        id
        bcId
        firstName
        lastName
        email
        phone
        role
        companyRoleId
        companyRoleName
    }
}
```

## Creating New Users

**Check user email example:**

```
query CheckUserEmail(
    $email: String!
) {
    userEmailCheck(
        email: $email
    ) {
        userType
    }
}
```

**Example response:**

```json
{
    "data": {
        "userEmailCheck": {
            "userType": 2
        }
    }
}
```

**Create user example:**

```
mutation CreateUser(
    $companyId: Int!,
    $email: String!,
    $firstName: String!,
    $lastName: String!,
    $role: Int
) {
    userCreate(
        userData: {
            companyId: $companyId,
            email: $email,
            firstName: $firstName,
            lastName: $lastName,
            role: $role
        }
    ) {
        user {
            id
            firstName
            lastName
            email
            role
            companyRoleId
            companyRoleName
        }
    }
}
```

## Custom User Roles

**Get roles example:**

```
query GetUserRoles(
    $companyId: Int!
) {
    companyRoles(
        companyId: $companyId
    ) {
        edges {
            node {
                id
                name
            }
        }
    }
}
```

**Example response:**

```json
{
    "data": {
        "companyRoles": {
            "edges": [
                {
                    "node": {
                        "id": "10009",
                        "name": "Admin"
                    }
                },
                {
                    "node": {
                        "id": "10010",
                        "name": "Senior Buyer"
                    }
                },
                {
                    "node": {
                        "id": "10011",
                        "name": "Junior Buyer"
                    }
                },
                {
                    "node": {
                        "id": "11223",
                        "name": "Custom Shopper Role"
                    }
                }
            ]
        }
    }
}
```

**Search example:**

```
query GetUserRoles(
    $companyId: Int!
) {
    companyRoles(
        companyId: $companyId,
        search: "Junior Buyer"
    ) {
        edges {
            node {
                id
                name
            }
        }
    }
}
```

**Example using company role ID:**

```
mutation CreateUser(
    $companyId: Int!,
    $email: String!,
    $firstName: String!,
    $lastName: String!,
    $companyRoleId: Int
) {
    userCreate(
        userData: {
            companyId: $companyId,
            email: $email,
            firstName: $firstName,
            lastName: $lastName,
            companyRoleId: $companyRoleId
        }
    ) {
        user {
            ...
        }
    }
}
```

**Example variables:**

```json
{
    "companyId": 1112233,
    "email": "johndoe@example.com",
    "firstName": "John",
    "lastName": "Doe",
    "companyRoleId": 10011
}
```

## Updating and Deleting Users

**Update user example:**

```
mutation UpdateUser(
    $companyId: Int!,
    $userId: Int!,
    $firstName: String,
    $lastName: String,
    $phone: String,
    $companyRoleId: Int
) {
    userUpdate(
        userData: {
            companyId: $companyId,
            userId: $userId,
            firstName: $firstName,
            lastName: $lastName,
            phone: $phone,
            companyRoleId: $companyRoleId
        }
    ) {
        user {
            id
            firstName
            lastName
            email
            role
            companyRoleId
            companyRoleName
        }
    }
}
```

**Delete user example:**

```
mutation DeleteUser(
    $companyId: Int!,
    $userId: Int!
) {
    userDelete(
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
        "userDelete": {
            "message": "Success"
        }
    }
}
```
