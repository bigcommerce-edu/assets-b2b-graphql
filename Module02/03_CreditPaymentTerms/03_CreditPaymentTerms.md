# Credit and Payment Terms

## Fetching Credit Configuration

**Get credit config example:**

```
query GetCreditConfig {
    companyCreditConfig {
        creditEnabled
        creditCurrency
        availableCredit
        limitPurchases
        creditHold
    }
}
```

**Example response:**

```json
{
    "data": {
        "companyCreditConfig": {
            "creditEnabled": true,
            "creditCurrency": "USD",
            "availableCredit": 5000.0,
            "limitPurchases": true,
            "creditHold": true
        }
    }
}
```

## Fetching Payment Terms

**Get payment terms example:**

```
query GetPaymentTerms(
    $companyId: Int!
) {
    companyPaymentTerms(
        companyId: $companyId
    ) {
        isEnabled
        paymentTerms
    }
}
```

**Example response:**

```json
{
    "data": {
        "companyPaymentTerms": {
            "isEnabled": false,
            "paymentTerms": 15
        }
    }
}
```

[Next](../04_Lab_PostmanRegistrationLogin/01_PostmanSetup/01_PostmanSetup.md)
