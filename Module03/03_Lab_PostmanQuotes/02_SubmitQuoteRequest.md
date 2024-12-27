# Create Quote

Authorization: Inherit auth from parent

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
mutation CreateQuote(
    $message: String,
    $subtotal: Decimal!,
    $discount: Decimal!,
    $grandTotal: Decimal!,
    $userEmail: String,
    $quoteTitle: String,
    $shippingAddress: ShippingAddressInputType!,
    $billingAddress: BillingAddressInputType!,
    $contactInfo: ContactInfoInputType!,
    $companyId: Int,
    $currency: CurrencyInputType!,
    $storeHash: String!,
    $productList: [ProductInputType]!
) {
    quoteCreate(
        quoteData: {
            message: $message,
            subtotal: $subtotal,
            discount: $discount,
            grandTotal: $grandTotal,
            userEmail: $userEmail,
            quoteTitle: $quoteTitle,
            shippingAddress: $shippingAddress,
            billingAddress: $billingAddress,
            contactInfo: $contactInfo,
            companyId: $companyId,
            currency: $currency,
            storeHash: $storeHash,
            productList: $productList
        }
    ) {
        quote {
            id
            createdAt
            quoteNumber
            quoteTitle
            quoteUrl
        }
    }
}
```

Variables:

```json
{
    "message": "I need this ASAP.",
    "subtotal": {{products_price_total}},
    "discount": 0,
    "grandTotal": {{products_price_total}},
    "userEmail": "{{b2b_logged_in_email}}",
    "quoteTitle": "Re-stock Quote",
    "shippingAddress": {
        "country": "United States",
        "state": "Texas",
        "city": "Austin",
        "zipCode": "78726",
        "address": "123 Park Central",
        "firstName": "John",
        "lastName": "Doe",
        "addressLine1": "123 Park Central",
        "phoneNumber": "444-555-6666"
    },
    "billingAddress": {
        "country": "United States",
        "state": "Texas",
        "city": "Austin",
        "zipCode": "78726",
        "address": "123 Park Central",
        "firstName": "John",
        "lastName": "Doe",
        "addressLine1": "123 Park Central",
        "phoneNumber": "444-555-6666"
    },
    "contactInfo": {
        "name": "John Doe",
        "email": "{{b2b_logged_in_email}}",
        "companyName": "John's Widgets",
        "phoneNumber": "111-222-3333"
    },
    "companyId": {{b2b_logged_in_company_id}},
    "currency": {},
    "storeHash": "{{store_hash}}",
    "productList": [
        {
            "productId": {{product1_id}},
            "sku": "{{product1_sku}}",
            "basePrice": {{product1_price}},
            "discount": 0,
            "offeredPrice": {{product1_price}},
            "quantity": 10,
            "variantId": {{product1_variant_id}},
            "imageUrl": "{{product1_image_url}}",
            "productName": "{{product1_name}}",
            "options": []
        },
        {
            "productId": {{product2_id}},
            "sku": "{{product2_sku}}",
            "basePrice": {{product2_price}},
            "discount": 0,
            "offeredPrice": {{product2_price}},
            "quantity": 10,
            "variantId": {{product2_variant_id}},
            "imageUrl": "{{product2_image_url}}",
            "productName": "{{product2_name}}",
            "options": []
        }
    ]
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

const quote = pm.response.json().data?.quoteCreate?.quote;
pm.test(`Response includes quote with ID`, () => {
    pm.expect(
        quote?.id
    ).to.be.a('string');
});
pm.test('Quote includes created date', () => {
    pm.expect(
        quote?.createdAt
    ).to.be.a('number');
});

pm.collectionVariables.unset('quote_id');
pm.collectionVariables.unset('quote_created_at');

pm.collectionVariables.set('quote_id', parseInt(quote?.id));
pm.collectionVariables.set('quote_created_at', quote?.createdAt);
```

[Next](./03_GetQuotes.md)