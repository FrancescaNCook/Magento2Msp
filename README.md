<p align="center">
  <img src="https://www.multisafepay.com/img/multisafepaylogo.svg" width="400px" position="center">
</p>

# MultiSafepay plugin for Magento 2 (deprecated)

:warning: This plugin is now deprecated and we are no longer providing support. Migrate to the [New Magento 2 plugin](https://github.com/MultiSafepay/magento2) as soon as possible. 

[![Latest stable version](https://img.shields.io/packagist/v/multisafepay/magento2msp.svg)](https://packagist.org/packages/multisafepay/magento2msp)
[![Total downloads](https://img.shields.io/packagist/dt/multisafepay/magento2msp.svg)](https://packagist.org/packages/multisafepay/magento2msp)
[![License](https://img.shields.io/packagist/l/multisafepay/magento2msp.svg)](https://github.com/MultiSafepay/Magento2Msp/blob/master/LICENSE.md)

## Payment methods
- Cards: All
- Banking methods: All
- Pay later: All
- Wallets: Alipay, Apple Pay, PayPal

## Requirements
- [MultiSafepay account](https://testmerchant.multisafepay.com/signup)
- Magento Open Source version 2.2.x & 2.3.x & 2.4.x 
- PHP 7.0 and higher
- Tested with PHP 7.0 (Magento 2.3.x adds support for 7.2)
- [Open Software License (OSL 3.0)](https://github.com/MultiSafepay/Magento2Msp/blob/master/LICENSE.md)

If you use [Magento Commerce](https://business.adobe.com/products/magento/magento-commerce.html), email <integration@multisafepay.com>

## Installation

You can install the plugin via Magento Marketplace, SFTP, or Composer. 

To install via Composer:
```shell
composer require multisafepay/magento2msp
php bin/magento setup:upgrade
php bin/magento setup:static-content:deploy
```

The latest stable release is downloaded and installed in your Magento 2 webshop.

Depending on your webserver/webshop configuration, you may also need to:

- Set the rights on files correctly. Our files can be found at vendor/multisafepay/magento2msp.
- Empty static files when running in production mode.
- Flush your cache.

## Configuration
1. Sign in to your Magento 1 backend. 
2. Go to **Stores** > **Configuration** > **MultiSafepay x.x.x** > **MultiSafepay settings**.  
3. Go to **Stores** > **(Settings) Configuration** > **MultiSafepay x.x.x** > **MultiSafepay gateways**. 

Make sure the selected payment methods are actived for your [MultiSafepay account](https://merchant.multisafepay.com).

## User guide

### Checkouts
Our plugin is compatible with most Magento 2 checkouts. However, we cannot guarantee that all checkouot features will function properly.

We test our plugin with two Magento 2 checkouts:  

- Magento 2 core checkout  
- [MagePlaza OneStepCheckout](https://www.mageplaza.com/magento-2-one-step-checkout-extension)

{{< blue-notice>}}**Note:** Always test OneStepCheckout before use to make sure it is compatible with your specific configuration of the plugin. {{< /blue-notice>}}

### Custom totals

From version 1.9.0 and higher, **all** order totals are automatically read and displayed on the MultiSafepay payment page.

Sometimes this results in unwanted custom totals appearing on payment pages.

**Excluding custom totals from payment pages**

1. Sign in to your Magento 2 backend.
2. Go to **Stores** > **Configuration** > **MultiSafepay** > **MultiSafepay settings** > **Advanced settings** > **Exclude custom totals**.

### Order lifetimes 

The default lifetime of **Pending payment** orders in Magento 2 is 480 minutes (8&nbsp;hours). For payment methods with a longer authorization period, the order status changes to **Cancelled** after 8 hours.

To extend the lifetime of pending payments orders, increase the **Order Cron settings** value to longer than the validation period.

For instructions, see Magento – [Pending payment order lifetime](https://docs.magento.com/user-guide/sales/order-pending-payment-lifetime.html).

**ERP systems**

For ERP systems, if the order status is **Declined**, successful payments often fail to process for orders with **Cancelled** status.

The lifetime of bank transfers is 86400 minutes (60 days).

The order status in Magento 2 changes to **Cancelled** before the payment can be matched to the order.

### Payment methods

By default, the selection of payment methods is not adjusted per country. To change this setting, follow these steps:

1. Sign in to your [backend](/glossaries/multisafepay-glossary/#backend).
2. Go to **Stores** > **Sales** > **Checkout** > **Checkout options**.
3. Locate **Display billing address on (store view)**.
4. Select **Payment page**.
5. Click **Save settings&&.

**Note:** This only applies to Magento 2.3.4 Community - Plugin 1.14.0.

A payment methods may not appear in your checkout if:

- An external gateway filter is activated, e.g. [Rico Neitzel Payment Filter](https://github.com/riconeitzel/PaymentFilter).
- There are configuration errors in your backend and/or in the database preventing changes  being saved.
- **Enabled in checkout** is unchecked for the **MultiSafepay payment method**.

### Payment links

To generate payment links in your backend, follow these steps:

1. Sign in to your Magento 2 backend.
2. Go to **Stores** > **MultiSafepay 1.x.x** > **MultiSafepay settings**.
3. Click **Create payment link**, and ensure that **Yes** is selected from the list.
4. Under **Sales** > **Orders** > **Create new order**, create an order as usual.
5. Under the **Orders overview**, select the order you just created.
6. Go to **Comments history**. The payment link appears under **Notes for this order**.
7. Copy and paste the link and send it to the customer.

As of version 1.7.0, we have added a feature to include payment links in order confirmation emails for orders created in the backend.

1. Go to **Marketing** > **Email templates**.
2. Add a template (import from "new order")
3. Add this *sample* code the template
    ````html
    {{depend order.getPayment().getAdditionalInformation('payment_link')}}
        <a href="{{var order.getPayment().getAdditionalInformation('payment_link')}}">Pay now with {{var order.getPayment().getAdditionalInformation('method_title')}}</a>
    {{/depend}}
    ````
4. Go to **Stores** > **Configuration** > **Sales** > **Sales emails**.
5. Replace the **New order confirmation template** with your template.
6. Test.

### Recurring payments

To process [recurring payments](/features/recurring-payments), enable tokenization via **Stores** > **Configuration** > **MultiSafepay** > **MultiSafepay settings**.

### Refunds 
You can refund orders or issue credit memos from your Magento 2 backend.  

1. Sign in to your Magento backend.
2. Go to the **Invoices** tab, and click the relevant MultiSafepay invoice.
3. Click **Credit memo**, and then click either:  
    - Offline refund: No refund request is sent to MultiSafepay.
    - Refund: A refund request is sent to MultiSafepay.

You can also refund from your [MultiSafepay dashboard](https://merchant.multisafepay.com).

**Note:** For orders with a Fooman surcharge, you cannot refund from your backend but you can from your [MultiSafepay dashboard](https://merchant.multisafepay.com).

### Surcharges

[Surcharges](/security-and-legal/payment-regulations/about-surcharges/) are no longer supported in the plugin.

**Fooman**

To apply a surcharge or payment fee to a payment method, you can use the third-party [Fooman](https://store.fooman.co.nz/extensions/magento2) package.

**Refunds**

You can refund orders with a Fooman surcharge applied in your [MultiSafepay dashboard](https://merchant.multisafepay.com), but not from your Magento 2 backend.  

### Updates

Run the following commands via the CLI:
```
composer update multisafepay/magento2msp 
php bin/magento setup:upgrade
```

Depending on your webserver/webshop configuration, you may also need to:

- Set the rights on files correctly. Our files can be found at vendor/multisafepay/magento2msp.
- Empty static files when running in production mode.
- Flush your cache.
 


