---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the PayForMe API
---

# Introduction

Welcome to the PayForMe API! You can use our API to access PayForMe API endpoints, which can be used to access and manage PayForMe orders and payment links.

We have language bindings in Shell which can be easily translated to other languages! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.


# Authentication

The PayForMe API uses API keys to authenticate requests. You can access your API keys in the [PayForMe Dashboard](https://#).

Your API keys must be kept secret because they carry many privileges. Do not expose your API keys publicly such as in your client-side code, GitHub, etc.

Your live api key is prefixed with `api_live` and your test mode api key is prefixed with `api_test`. For e.g. a full key will be similar to `api_live_123456abcdef`.


HTTPS Bearer Auth is used for authentication. Provide your API key as the Bearer token. 
If you need to authenticate via bearer auth (e.g., for a cross-origin request), use `-H "Authorization: Bearer api_live_123456abcdef"` 

All API requests must be made over HTTPS. Calls made over plain HTTP will fail. API requests without authentication will also fail.

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "https://api.payforme.io" \
  -H "Authorization: Bearer api_live_123456abcdef"
```
> Make sure to replace `api_live_123456abcdef` with your API key.

PayForMe API expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: Bearer api_live_123456abcdef`


<aside class="notice">
You must replace <code>api_live_123456abcdef</code> with your personal API key.
</aside>

# Checkout Page

The checkout page allows the buyer to include a shipping address and finalize the checkout process by generating a payment link themselves. The payment link can then be shared with anyone to pay.

## Redirect to the Checkout Page

This endpoint to redirects to the checkout page on the brower. Call this endpoint for exampe when the user clicks on a Buy or Checkout Button on your ecommerce website.


```shell
curl "https://api.payforme.io/checkoutPage" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('YOUR_API_KEY');
let kittens = api.kittens.get();
```

> The above command redirects to the checkout page URL smiliar to this:

```
https://api.payforme.io/checkout-page?ref=<token>
```

### HTTP Request

`POST https://api.payforme.io/checkoutPage`

### Body Parameters

Parameter | Default | Description
--------- | ------- | -----------
merchant_order_id | default | If set uniquely identifies the order in the merchant's system.
needs_shipping_address | true | If set to false, buyer's shipping address will not be required to fulfil the order.
shipping_address | undefined | required unless `needs_shipping_address` is set to false.
amount | n/a | required field must be set to determine the amount to charge the purchasing customer. 
line_items | undefined | Optional - if set displays a breakdown of the items being purchased. This is used to display items only. The API caller is responsible for making sure line_items add up to the  `amount` charged. 

<aside class="success">
Remember — Always keep your API keys secret!
</aside>

## Get the Checkout Page URL

Gets the checkout page URL instead of redirecting to it. 

```shell
curl "https://api.payforme.io/checkoutPage/abc" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('YOUR_API_KEY');
let max = api.kittens.get('abc');
```

> The above command returns JSON structured like this:

```json
{
  "id": abc,
  "object": checkout-page,
  "url": "https://api.payforme.io/checkout-page?ref=abc",
  "expires": 123456789076
}
```

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET https://api.payforme.io/checkoutPage/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the checkout order to retrieve

## Disable a Checkout Order

Disables or invalidates a specified checkout order.


```shell
curl "https://api.payforme.io/<payforme_order_id>" \
  -X DELETE \
  -H "Authorization: Bearer YOUR_API_KEY"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('YOUR_API_KEY');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```


### HTTP Request

`DELETE https://api.payforme.io/checkoutPage/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the PayForMe order to invalidate

# Payment Link

The payment link is a URL that allows allows anyone with the link to pay. The Payment Link contains details about the order such as the items being purchased and the buyer's shipping info. Anyone with the payment link can pay on behalf of the buyer. Once payment is received, the merchant is notified to fulfill the order. The buyer and payer also get notified.

## Create a Payment Link


```shell
curl "https://api.payforme.io/paymentLink" \
  -H "Authorization: Bearer api_live_123456abcdef"
```


> The above command returns JSON structured like this:

```json
{
  "id": abcdef,
  "url": "https://pay.payforme.io/abcdef",
  "expires": 123456789076
}
```

This endpoint creates a payment link.

### HTTP Request

`POST https://api.payforme.io/paymentLink`

### Body Parameters

Parameter | Default | Description
--------- | ------- | -----------
merchant_order_id | default | If set uniquely identifies the order in the merchant's system.
needs_shipping_address | true | If set to false, buyer's shipping address will not be required to fulfil the order.
shipping_address | undefined | required unless `needs_shipping_address` is set to false.
amount | n/a | required field must be set to determine the amount to charge the purchasing customer. 
line_items | undefined | Optional - if set displays a breakdown of the items being purchased. This is used to display items only. It helps the payer review items being purchased. used to . The API caller is responsible for making sure line_items add up to the  `amount` charged. 

<aside class="success">
Remember — Always keep your API keys secret!
</aside>
