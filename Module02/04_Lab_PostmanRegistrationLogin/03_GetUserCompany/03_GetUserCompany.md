# Get User Company

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
    }
}
```

Variables:

```json
{
    "userId": {{b2b_logged_in_id}}
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

const userCompany = pm.response.json().data?.userCompany;
pm.test('Response includes a company ID', () => {
    pm.expect(
        userCompany?.id
    ).to.be.a('string');
});

pm.environment.unset('b2b_logged_in_company_id');
pm.environment.set('b2b_logged_in_company_id', parseInt(userCompany?.id));
```

[Next](../../05_Lab_PostmanCompanyManagement/01_GetCompanyAddresses/01_GetCompanyAddresses.md)