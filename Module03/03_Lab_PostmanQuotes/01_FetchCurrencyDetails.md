# Get Currencies

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
query GetCurrencies(
    $storeHash: String!,
    $channelId: String!
) {
	currencies(
		storeHash: $storeHash,
		channelId: $channelId,
	) {
		currencies {
			currency_code,
			currency_exchange_rate,
			token,
			decimal_token,
			decimal_places,
			token_location,
			thousands_token,
		},
		channelCurrencies
	}
}
```

Variables:

```json
{
    "storeHash": "{{store_hash}}",
    "channelId": "{{storefront_channel_id}}"
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

const currencies = pm.response.json().data?.currencies?.currencies ?? [];
pm.test(`Response includes at least one currency`, () => {
    pm.expect(
        currencies.length
    ).to.be.greaterThan(0);
});
const defaultCurrencyCode = pm.response.json().data?.currencies?.channelCurrencies?.default_currency;
pm.test(`Response includes default currency code`, () => {
    pm.expect(
        defaultCurrencyCode
    ).to.be.a('string');
});
const defaultCurrency = currencies.find(currency => {
    return currency?.currency_code === defaultCurrencyCode;
});
pm.test(`Response includes default currency info`, () => {
    pm.expect(
        defaultCurrency?.currency_code
    ).to.be.a('string');
});

pm.collectionVariables.unset('currency_code');
pm.collectionVariables.unset('currency_exchange_rate');
pm.collectionVariables.unset('currency_token');
pm.collectionVariables.unset('currency_decimal_token');
pm.collectionVariables.unset('currency_decimal_places');
pm.collectionVariables.unset('currency_token_location');
pm.collectionVariables.unset('currency_thousands_token');

pm.collectionVariables.set('currency_code', defaultCurrency.currency_code);
pm.collectionVariables.set('currency_exchange_rate', defaultCurrency.currency_exchange_rate);
pm.collectionVariables.set('currency_token', defaultCurrency.token);
pm.collectionVariables.set('currency_decimal_token', defaultCurrency.decimal_token);
pm.collectionVariables.set('currency_decimal_places', defaultCurrency.decimal_places);
pm.collectionVariables.set('currency_token_location', defaultCurrency.token_location);
pm.collectionVariables.set('currency_thousands_token', defaultCurrency.thousands_token);
```

[Next](./02_FetchProductDetails.md)