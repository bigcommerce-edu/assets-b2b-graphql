# Companies and Addresses

## Company Registration

**Create customer mutation:**

```
mutation CreateCustomer(
    $storeHash: String!,
    $email: String!,
    $password: String,
    $firstName: String!,
    $lastName: String!,
    $phone: String,
    $channelId: Int
) {
    customerCreate(
        customerData: {
            storeHash: $storeHash,
            email: $email,
            firstName: $firstName,
            lastName: $lastName,
            phone: $phone,
            originChannelId: $channelId,
            channelIds: [$channelId],
            authentication: {
                forcePasswordReset: false,
                newPassword: $password
            }
        }
    ) {
        customer {
            id
            email
        }
    }
}
```

**Example variables:**

```json
{
    "storeHash": "aaaaabbbbb",
    "email": "johndoe@example.com",
    "password": "Password123",
    "firstName": "John",
    "lastName": "Doe",
    "phone": "111-222-3333",
    "channelId": 1
}
```

**Example response:**

```json
{
    "data": {
        "customerCreate": {
            "customer": {
                "id": 5,
                "email": "johndoe@example.com"
            }
        }
    }
}
```

**Note:** The `registerCustomer` mutation in the BigCommerce GraphQL Storefront API can be used instead of `customerCreate`. See [Authentication](../01_Overview/02_Authentication.md) for the credentials required with that API.

**Create company mutation:**

Both `customerId` and `customerEmail` are required to use the `companyCreate` mutation.

```
mutation CreateCompany(
    $storeHash: String!,
    $customerId: String!,
    $customerEmail: String!,
    $companyName: String!,
    $companyEmail: String,
    $addressLine1: String!,
    $city: String!,
    $country: String!,
    $state: String!,
    $zipCode: String!
) {
    companyCreate(
        companyData: {
            storeHash: $storeHash,
            customerId: $customerId,
            customerEmail: $customerEmail,
            companyName: $companyName,
            companyEmail: $companyEmail,
            addressLine1: $addressLine1,
            city: $city,
            country: $country,
            state: $state,
            zipCode: $zipCode
        }
    ) {
        company {
            id
            companyName
        }
    }
}
```

**Example variables:**

```json
{
    "storeHash": "aaaaabbbbb",
    "customerId": "27",
    "customerEmail": "johndoe@example.com",
    "companyName": "John's Widgets",
    "companyEmail": "johnswidgets@example.com",
    "addressLine1": "123 Park Central",
    "city": "Austin",
    "country": "United States",
    "state": "Texas",
    "zipCode": "78726"
}
```

**Example response:**

```json
{
    "data": {
        "companyCreate": {
            "company": {
                "id": "1112233",
                "companyName": "John's Widgets"
            }
        }
    }
}
```

**Note:** The `registerCompany` mutation in the BigCommerce GraphQL Storefront API can be used instead of `companyCreate`. Unlike `companyCreate`, `registerCompany` requires a logged-in customer context. See [Authentication](../01_Overview/02_Authentication.md) for details.

**Example login mutation:**

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
                    "id": "99988877",
                    "bcId": 27,
                    "firstName": "John",
                    "lastName": "Doe",
                    "email": "john@example.com"
                }
            }
        }
    }
}
```

**Get user company example:**

```
query GetUserCompany(
    $userId: Int!
) {
    userCompany(
        userId: $userId
    ) {
        id
        companyName
        companyStatus
        addressLine1
        addressLine2
        city
        state
        zipCode
        country
        customerGroupId
    }
}
```

## Managing Addresses

**Create address mutation:**

```
mutation CreateAddress(
    $companyId: Int!,
    $firstName: String!,
    $lastName: String!,
    $addressLine1: String!,
    $addressLine2: String,
    $country: String!,
    $countryCode: String!,
    $state: String!,
    $stateCode: String,
    $city: String!,
    $zipCode: String!,
    $phoneNumber: String,
    $isShipping: Int,
    $isBilling: Int,
    $isDefaultShipping: Int,
    $isDefaultBilling: Int,
    $label: String,
    $company: String
) {
    addressCreate(
        addressData: {
            companyId: $companyId,
            firstName: $firstName,
            lastName: $lastName,
            addressLine1: $addressLine1,
            addressLine2: $addressLine2,
            country: $country,
            countryCode: $countryCode,
            state: $state,
            stateCode: $stateCode,
            city: $city,
            zipCode: $zipCode,
            phoneNumber: $phoneNumber,
            isShipping: $isShipping,
            isBilling: $isBilling,
            isDefaultShipping: $isDefaultShipping,
            isDefaultBilling: $isDefaultBilling,
            label: $label,
            company: $company
        }
    ) {
        address {
            id
            label
            isShipping
            isBilling
            isDefaultShipping
            isDefaultBilling
        }
    }
}
```

**Example variables:**

```json
{
    ...
    "isShipping": 1,
    "isBilling": 0,
    "isDefaultShipping": 1,
    "isDefaultBilling": 0,
    "label": "Main Shipping Address"
}
```

**Fetch addresses example:**

```
query GetAddresses(
    $companyId: Int!
) {
    addresses(
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
                firstName
                lastName
                isShipping
                isDefaultShipping
                isBilling
                isDefaultBilling
                addressLine1
                addressLine2
                city
                state
                stateCode
                country
                countryCode
                zipCode
                phoneNumber
                label
            }
        }
    }
}
```

**Example response:**

```json
{
    "data": {
        "addresses": {
            "pageInfo": {
                "hasNextPage": true,
                "hasPreviousPage": false,
                "startCursor": "YXJy...",
                "endCursor": "YXJy..."
            },
            "totalCount": 12,
            "edges": [
                {
                    "node": {
                        "id": "11122233",
                        "firstName": "John",
                        "lastName": "Doe",
                        "isShipping": 1,
                        "isDefaultShipping": 0,
                        "isBilling": 1,
                        "isDefaultBilling": 0,
                        "addressLine1": "123 Park Central",
                        "addressLine2": "Ste 3",
                        "city": "Austin",
                        "state": "Texas",
                        "stateCode": "TX",
                        "country": "United States",
                        "countryCode": "US",
                        "zipCode": "78726",
                        "phoneNumber": "111-222-3333",
                        "label": "Main Billing Address"
                    }
                },
                ...
            ]
        }
    }
}
```

**Get address example:**

```
query GetAddresses(
    $companyId: Int!,
    $addressId: Int!
) {
    address(
        companyId: $companyId,
        addressId: $addressId
    ) {
        id
        firstName
        lastName
        isShipping
        isDefaultShipping
        isBilling
        isDefaultBilling
        addressLine1
        addressLine2
        city
        state
        stateCode
        country
        countryCode
        zipCode
        phoneNumber
        label
    }
}
```

**Update address example:**

```
mutation UpdateAddress(
    $companyId: Int!,
    $addressId: Int!,
    $firstName: String!,
    $lastName: String!,
    $addressLine1: String!,
    $addressLine2: String,
    $country: String!,
    $countryCode: String!,
    $state: String!,
    $stateCode: String,
    $city: String!,
    $zipCode: String!,
    $phoneNumber: String,
    $isShipping: Int,
    $isBilling: Int,
    $isDefaultShipping: Int,
    $isDefaultBilling: Int,
    $label: String,
    $company: String
) {
    addressUpdate(
        addressData: {
            companyId: $companyId,
            addressId: $addressId,
            firstName: $firstName,
            lastName: $lastName,
            addressLine1: $addressLine1,
            addressLine2: $addressLine2,
            country: $country,
            countryCode: $countryCode,
            state: $state,
            stateCode: $stateCode,
            city: $city,
            zipCode: $zipCode,
            phoneNumber: $phoneNumber,
            isShipping: $isShipping,
            isBilling: $isBilling,
            isDefaultShipping: $isDefaultShipping,
            isDefaultBilling: $isDefaultBilling,
            label: $label,
            company: $company
        }
    ) {
        address {
            id
            label
            isShipping
            isBilling
            isDefaultShipping
            isDefaultBilling
        }
    }
}
```

**Delete address example:**

```
mutation DeleteAddress(
    $companyId: Int!,
    $addressId: Int!
) {
    addressDelete(
        companyId: $companyId,
        addressId: $addressId
    ) {
        message
    }
}
```

**Example response:**

```json
{
    "data": {
        "addressDelete": {
            "message": "Success"
        }
    }
}
```
