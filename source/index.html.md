---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - javascript

toc_footers:
- <a href='https://app.payforme.io/dashboard' target='_blank'>Sign Up for a Developer Key</a>
- <a href='https://payforme.io' target='_blank'>Go to PayForMe Homepage</a>

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


HTTPS Bearer Auth is used for authentication. Provide your API key as the Bearer token. For example  `-H "Authorization: Bearer api_live_123456abcdef"` 

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
Don't forget to replace <code>api_live_123456abcdef</code> with your personal API key.
</aside>

# Checkout Page

Merchants can redirect customers to the checkout page to complete the checkout/payment process. This process allows the buyer to update their shipping information and generate a payment link that can be shared with anyone to pay.

## Redirect to the Checkout Page

This endpoint redirects the customer to the checkout page on the browser. A good time to call this endpoint is when a customer clicks on a Buy or Checkout Button on an ecommerce website.


```shell
curl "https://api.payforme.io/checkoutPage" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('YOUR_API_KEY');
let kittens = api.kittens.get();
```

> The above command redirects to a checkout page URL that looks smiliar to this:

```
https://api.payforme.io/checkout-page?ref=<token>
```

### HTTP Request

`POST https://api.payforme.io/checkoutPage`

### Body Parameters

Parameter | Default | Description
--------- | ------- | -----------
merchant_order_id | "default" | If set uniquely identifies the order in the merchant's system.
needs_shipping_address | true | If set to false, buyer's shipping address won't be required to fulfil the order.
shipping_address | undefined | Optional if  `needs_shipping_address` is set to false. 
amount | undefined | Required - must be set to determine the amount to charge the purchasing customer. 
line_items | undefined | Optional - if set displays a breakdown of the items being purchased. This is used to display items only. The API caller is responsible for making sure line_items add up to the  `amount` charged. 
redirect_to_checkout_page | true | Set to false to return the checkout page URL instead of redirecting to it.
meta_data | undefined | Optional - If set is returned back exactly in PayForMe Order data. It can be used to store additional info needed by the merchant.

<aside class="success">
Remember — Always keep your API keys secret!
</aside>

## Get the Checkout Page URL

Gets the checkout page URL instead of redirecting to it. Once you have the URL, the customer needs needs to visit the URL in a web browser to complete the checkout process. The checkout URL is not the same as a <a href='#payment-link'>Payment Link</a>. 

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
  "id": "abc",
  "object": "checkout-page",
  "url": "https://api.payforme.io/checkout-page?ref=abc",
  "expires": 123456789076
}
```

<aside class="warning">Uses the same API for Checkout Page Redirect. However you need to set "redirect_to_checkout_page" param to false when calling the checkoutPage API.</aside>

### HTTP Request

`POST https://api.payforme.io/checkoutPage`

### Body Parameters

Parameter | Default | Description
--------- | ------- | -----------
redirect_to_checkout_page | true | Set to false to return the checkout page URL instead of redirecting to it.



# Payment Link

The payment link is a URL that allows anyone with the link to pay. The Payment Link contains details about the order such as the items being purchased and the buyer's shipping info. The buyer's shipping info is kept private and only shared with the merchant to fulfill the order. 


Anyone with the payment link can pay on behalf of the buyer. Once payment is received, the merchant is notified to fulfill the order. The buyer and payer also get notified.

## Create a Payment Link


```shell
curl "https://api.payforme.io/paymentLink" \
  -H "Authorization: Bearer api_live_123456abcdef"
```


> The above command returns JSON structured like this:

```json
{
  "id": "abcdef",
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
meta_data | undefined | Optional - If set is returned back exactly in PayForMe Order data. It can be used to store additional info needed by the merchant.

<aside class="success">
Remember — Always keep your API keys secret!
</aside>
