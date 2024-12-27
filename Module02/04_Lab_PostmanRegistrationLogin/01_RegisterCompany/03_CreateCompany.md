# Create a Company

Authorization: No Auth

Request method:
POST

URL:
```
https://api-b2b.bigcommerce.com/graphql
```

Headers:

| Header       | Value            |
|--------------|------------------|
| Accept       | application/json |
| Content-Type | application/json |

Body:

```
mutation CreateCompany(
    $storeHash: String!,
    $customerId: String!,
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

Variables:

```json
{
    "storeHash": "{{store_hash}}",
    "customerId": "{{admin_customer_id}}",
    "companyName": "John's Widgets",
    "companyEmail": "johndoe@example.com",
    "addressLine1": "987 Park Central East",
    "city": "Austin",
    "country": "United States",
    "state": "Texas",
    "zipCode": "78726"
}
```

Post-response Script:
```javascript
pm.test('Response is not an error', () => {
    pm.response.to.not.be.error;
    pm.response.to.not.have.jsonBody("errors");
});
pm.test('Response is JSON with data', () => {
    pm.response.to.have.jsonBody("data");
});


const company = pm.response.json().data?.companyCreate?.company;
pm.test('Response includes a company ID', () => {
    pm.expect(
        company?.id
    ).to.be.a('string');
});
```

[Next](../02_Login/01_AdminLogin.md)