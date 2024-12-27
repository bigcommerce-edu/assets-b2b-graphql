# Create Shipping Address

Authorization: 
Inherit auth from parent

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

Variables:

```json
{
    "companyId": {{b2b_logged_in_company_id}},
    "firstName": "Jack",
    "lastName": "Doe",
    "addressLine1": "456 Downtown Lane",
    "addressLine2": "Ste 1",
    "country": "United States",
    "countryCode": "US",
    "state": "Texas",
    "stateCode": "TX",
    "city": "Austin",
    "zipCode": "78726",
    "phoneNumber": "777-888-9999",
    "isShipping": 1,
    "isBilling": 0,
    "isDefaultShipping": 1,
    "isDefaultBilling": 0,
    "label": "Main Shipping",
    "company": "John's Widgets"
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

const address = pm.response.json().data?.addressCreate?.address;
pm.test('Response includes address with ID', () => {
    pm.expect(
        address?.id
    ).to.be.a('string');
});

pm.collectionVariables.unset('address_id');
pm.collectionVariables.set('address_id', address?.id);
```

[Next](../03_UpdateDeleteAddress/01_UpdateAddress.md)