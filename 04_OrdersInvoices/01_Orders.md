# Orders

## Fetching Company Orders

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
                companyId {
                    id
                    companyName
                }
                totalIncTax
                currencyCode
                items
                poNumber
                status
                shippingAddress
                shipments
                products
                customer
                companyName
                firstName
                lastName
            }
        }
    }
}
```

**Example response:**

```json
{
    "data": {
        "allOrders": {
            "pageInfo": {
                "hasNextPage": true,
                "hasPreviousPage": false,
                "startCursor": "YXJy...",
                "endCursor": "YXJy..."
            },
            "totalCount": 20,
            "edges": [
                {
                    "node": {
                        "id": "44455566",
                        "bcOrderId": 120,
                        "createdAt": 1724273249,
                        "userId": 99988877,
                        "bcCustomerId": 27,
                        "companyId": {
                            "id": "1112233",
                            "companyName": "John's Widgets"
                        },
                        "totalIncTax": 325.52,
                        "currencyCode": "USD",
                        "items": 2,
                        "poNumber": "12345",
                        "status": "Shipped",
                        "shippingAddress": "[{\"id\": 11, \"zip\": \"78726\", \"city\": \"Austin\",...}]",
                        "shipments":"[{\"id\": 2, \"items\": [{\"quantity\": 1, \"product_id\": 20,...}],...}]",
                        "products": "[{\"id\": 20, \"sku\": \"AWE-T\", ...}]",
                        "customer": "{\"id\": 27, \"email\": \"johndoe@example.com\",...}",
                        "companyName": "John's Widgets",
                        "firstName": "John",
                        "lastName": "Doe"
                    }
                },
                ...
            ]
        }
    }
}
```

## Fetching a Single Order

**Get order example:**

```
query GetOrder(
    $id: Int!
) {
    order(
        id: $id
    ) {
        id
        firstName
        lastName
        status
        customerId
        dateCreated
        subtotalExTax
        subtotalIncTax
        subtotalTax
        baseShippingCost
        shippingCostExTax
        shippingCostIncTax
        totalTax
        discountAmount
        totalExTax
        totalIncTax
        itemsTotal
        itemsShipped
        paymentMethod
        poNumber
        products
        billingAddress
        shippingAddress
        shipments
        invoiceId
        orderHistoryEvent {
            id
            eventType
            status
        }
    }
}
```

**Example variables:**

```json
{
    "id": 120
}
```

**Example response:**

```json
{
    "data": {
        "order": {
            "id": "120",
            "firstName": "John",
            "lastName": "Doe",
            "status": "Shipped",
            "customerId": 27,
            "dateCreated": "Wed, 21 Aug 2025 20:47:25 +0000",
            "subtotalExTax": "341.0000",
            "subtotalIncTax": "363.1800",
            "subtotalTax": "22.1800",
            "baseShippingCost": "9.0000",
            "shippingCostExTax": "9.0000",
            "shippingCostIncTax": "10.1700",
            "totalTax": "25.5200",
            "discountAmount": "50.0000",
            "totalExTax": "300.0000",
            "totalIncTax": "325.5200",
            "itemsTotal": 2,
            "itemsShipped": 2,
            "paymentMethod": "Purchase Order",
            "poNumber": "12345",
            "products": [
                {
                    "id": 20,
                    "sku": "AWE-T",
                    ...
                    "quantity": 1,
                    "price_tax": "6.2350",
                    "return_id": 0,
                    "total_tax": "3.7200",
                    "base_price": "89.0000",
                    "base_total": "89.0000",
                    ...
                    "product_id": 20,
                    ...
                    "price_ex_tax": "89.0000",
                    "total_ex_tax": "89.0000",
                    ...
                    "price_inc_tax": "95.2350",
                    "refund_amount": "0.0000",
                    "total_inc_tax": "95.2350",
                    ...
                    "quantity_shipped": 1,
                    ...
                    "applied_discounts": [
                        {
                            "id": "manual-discount",
                            "code": null,
                            "name": "Manual Discount",
                            "amount": "35.9000",
                            "target": "order"
                        }
                    ],
                    ...
                    "discounted_total_inc_tax": "56.8200",
                    ...
                },
                ...
            ],
            "billingAddress": {
                "zip": "78726",
                "city": "Austin",
                ...
            },
            "shippingAddress": [
                {
                    "id": 11,
                    "zip": "78726",
                    "city": "Austin",
                    ...
                }
            ],
            "shipments": [
                {
                    "id": 2,
                    "items": [
                        {
                            "quantity": 1,
                            "product_id": 20,
                            "order_product_id": 18
                        },
                        ...
                    ],
                    ...
                    "shipping_method": "Flat Rate",
                    "tracking_number": "123456789",
                    ...
                }
            ],
            "invoiceId": 7778899,
            "orderHistoryEvent": [
                {
                    "id": 11111111,
                    "eventType": 2,
                    "status": "Shipped"
                },
                {
                    "id": 22222222,
                    "eventType": 2,
                    "status": "Awaiting Payment"
                }
            ]
        }
    }
}
```

## Inspecting Ordered Products

**Get ordered products example:**

```
query GetOrderedProducts {
    orderedProducts {
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
                productId
                productName
                sku
                variantSku
                variantId
                optionList
                optionSelections
                orderedTimes
                firstOrderedAt
                lastOrderedAt
                imageUrl
                productUrl
            }
        }
    }
}
```

**Example response:**

```json
{
    "data": {
        "orderedProducts": {
            "pageInfo": {
                "hasNextPage": true,
                "hasPreviousPage": false,
                "startCursor": "YXJy...",
                "endCursor": "YXJy..."
            },
            "totalCount": 50,
            "edges": [
                {
                    "node": {
                        "id": "T3Jk...",
                        "productId": "20",
                        "productName": "Awesome T-Shirt",
                        "sku": "AWE-T",
                        "variantSku": "AWE-T-SM-R",
                        "variantId": "17",
                        "optionList": [
                            {
                                "id": 8,
                                "option_id": 3,
                                "order_product_id": 36,
                                "product_option_id": 111,
                                ...
                            },
                            ...
                        ],
                        "optionSelections": [
                            {
                                "option_id": 100,
                                "value_id": 1000
                            },
                            {
                                "option_id": 101,
                                "value_id": 2000
                            }
                        ],
                        "orderedTimes": "3",
                        "firstOrderedAt": 1724274130,
                        "lastOrderedAt": 1759804358,
                        "imageUrl": "https://cdn11.bigcommerce.com/...",
                        "productUrl": "/awesome-tshirt/"
                    }
                },
                ...
            ]
        }
    }
}
```
