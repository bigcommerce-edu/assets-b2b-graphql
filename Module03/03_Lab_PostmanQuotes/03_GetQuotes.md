# Get Quotes

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
query GetQuotes(
    $limit: Int,
    $after: String,
    $orderBy: String
) {
    quotes(
        first: $limit,
        after: $after,
        orderBy: $orderBy
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
                createdAt
                quoteNumber
                quoteTitle
                createdBy
                createdByEmail
                expiredAt
                subtotal
                discount
                discountType
                discountValue
                shippingTotal
                taxTotal
                grandTotal
                totalAmount
                orderId
                bcOrderId
                contactInfo
                trackingHistory
                notes
                legalTerms
                shippingMethod
                billingAddress
                shippingAddress
                salesRepInfo {
                    salesRepName
                    salesRepEmail
                }
                productsList {
                    productId
                    variantId
                    sku
                    basePrice
                    discount
                    offeredPrice
                    quantity
                    imageUrl
                    productName
                    options
                    notes
                    productUrl
                    type
                }
                quoteUrl
            }
        }
    }
}
```

Variables:

```json
{
    "limit": 5,
    "after": null,
    "orderBy": "-createdAt"
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

const result = pm.response.json().data?.quotes;
pm.test('Total count includes at least one quote', () => {
    pm.expect(
        result?.totalCount
    ).to.be.greaterThan(0);
});

let firstQuote = null;
if (Array.isArray(result?.edges)) {
    firstQuote = result.edges[0]?.node;
}
pm.test('Response includes at least one quote with an ID', () => {
    pm.expect(
        firstQuote?.id
    ).to.be.a('string');
});
```

[Next](./04_ExportQuotePdf.md)