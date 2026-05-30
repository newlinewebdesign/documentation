---
title: "Sogecommerce — Société Générale Hosted Payment Page"
sidebar_label: "Sogecommerce (Société Générale)"
sidebar_position: 92
description: "How to configure the Sogecommerce payment gateway plugin for J2Commerce. Covers store identifier, test and production keys, HMAC-SHA-256 signature, IPN notification URL, payment page options, selective 3DS, order status mapping, and troubleshooting."
---

# Sogecommerce — Société Générale Hosted Payment Page

Sogecommerce is the online payment gateway operated by Société Générale, France's third-largest bank. It runs on the Lyra Network V2 platform — the same foundation shared by PayZen and Systempay. Customers are redirected from your J2Commerce checkout to a Sogecommerce-hosted, PCI-compliant payment page to enter their card details. After payment, Sogecommerce sends a server-to-server notification (IPN) to confirm the result and redirects the buyer back to your store.

Card data never passes through your server. This gives you the lowest PCI compliance burden (SAQ-A).

This plugin is a separate add-on available from the [J2Commerce Extensions Store](https://www.j2commerce.com). It is not included with the core J2Commerce component.

---

## What's new in J2Commerce 6

The J2Commerce 6 version of this plugin has been rebuilt on native Joomla 6 MVC — no legacy FOF framework, no jQuery. Key improvements over the J2Store 4 version:

- **Configurable order-status mapping** — choose which J2Commerce status is set for confirmed, pending, cancelled, and failed payments.
- **Optional payment surcharge** — add a percentage fee, a fixed fee, or both, with optional tax.
- **Geozone restriction** — show this payment method only to buyers in specific countries or regions.
- **Amount restrictions** — hide the method for orders below a minimum or above a maximum amount.
- **Replay guard** — duplicate IPN callbacks are silently ignored; the order is never double-processed.

---

## How it works

1. The buyer reaches the payment step and selects Sogecommerce.
2. J2Commerce builds a signed form containing the order details and HMAC-SHA-256 signature and posts it to the Sogecommerce payment page.
3. The buyer enters card details on Sogecommerce's secure, hosted page.
4. Sogecommerce sends a server-to-server IPN to your store's notification URL and updates the order status.
5. Sogecommerce redirects the buyer back to your confirmation page.

---

## Prerequisites

- An active Sogecommerce merchant account with Société Générale
- Access to the Sogecommerce Back Office
- Your 8-digit **Store identifier** and your **Test key** and **Production key** (available inside the Back Office)
- The **signature algorithm** configured in the Back Office must match the algorithm set in the plugin

---

## Installation

This plugin is a separate add-on and is not included with J2Commerce.

1. Purchase and download the `plg_j2commerce_payment_sogecommerce.zip` package from the J2Commerce website.
2. Go to **System** -> **Install** -> **Extensions**.
3. Upload the `plg_j2commerce_payment_sogecommerce.zip` file.
4. The plugin installs and enables automatically.

To configure the plugin, go to **J2Commerce** -> **Payments** -> **Payment Methods** and click on **Sogecommerce**.

---

## Configuration

### Gateway access

These credentials come directly from your Sogecommerce Back Office.

<!-- SCREENSHOT: Plugin configuration screen showing the "Payment gateway access" section with Store identifier, Test key, Production key, Mode switcher, and Signature algorithm fields -->

| Field | Description | Default |
|-------|-------------|---------|
| **Store identifier** | Your 8-digit merchant site identifier provided by Sogecommerce. | `12345678` (example only) |
| **Key in test mode** | The certificate key for the test environment. Find it in the Back Office under your site's settings. | — |
| **Key in production mode** | The certificate key for the live environment. Available in the Back Office once production mode is activated. | — |
| **Mode** | Switch between **TEST** and **PRODUCTION**. Use TEST while verifying your setup; switch to PRODUCTION for live orders. | TEST |
| **Signature algorithm** | The algorithm used to sign the payment form. Must **exactly match** the algorithm configured in your Sogecommerce Back Office. HMAC-SHA-256 is strongly recommended; SHA-1 is available for older accounts. | HMAC-SHA-256 |
| **Payment page URL** | The URL of the Sogecommerce hosted payment page. Change only if Sogecommerce instructs you to use a different endpoint. | `https://sogecommerce.societegenerale.eu/vads-payment/` |

### CRITICAL: Copy the IPN URL into the Sogecommerce Back Office

The Instant Payment Notification (IPN) URL is how Sogecommerce confirms payment results to your store. Without it, orders will not be updated when payments complete.

The IPN URL for your store is:

```
index.php?option=com_j2commerce&task=checkout.confirmPayment&orderpayment_type=payment_sogecommerce
```

Prepend your site's domain to get the full URL — for example:

```
https://www.yourstore.com/index.php?option=com_j2commerce&task=checkout.confirmPayment&orderpayment_type=payment_sogecommerce
```

**Where to paste it in the Back Office:**

1. Log into your Sogecommerce Back Office.
2. Go to **Settings** -> **Notification rules**.
3. Open the **Instant payment notification at the end of the payment** rule.
4. Paste the full URL into the **Notification URL in test mode** field and the **Notification URL in production mode** field.
5. Save the rule.

<!-- SCREENSHOT: Sogecommerce Back Office — Notification rules screen showing the URL pasted into both test and production notification URL fields -->

If the IPN URL is not saved in the Back Office, Sogecommerce will not call your site after payment. Orders will remain in their pre-payment status. The buyer return URL alone is not a reliable update path because the buyer may close the browser before returning.

---

### Payment page

These settings control the appearance and behaviour of the hosted payment page.

| Field | Description | Default |
|-------|-------------|---------|
| **Default language** | The language shown on the payment page. The plugin automatically tries to match the visitor's Joomla language; this setting is the fallback. Supported: German, English, Chinese, Spanish, French, Italian, Japanese, Dutch, Polish, Portuguese, Russian, Swedish, Turkish. | French (fr) |
| **Available languages** | Restrict which languages the buyer can switch to on the payment page. Leave empty to allow all supported languages. | (all) |
| **Capture delay** | Number of days between authorisation and bank capture. Leave empty to use the default configured in your Back Office. | (Back Office default) |
| **Validation mode** | **Sogecommerce Back Office configuration** uses the mode set in the Back Office. **Automatic** captures funds immediately after authorisation. **Manual** requires you to log into the Back Office and confirm each payment before funds are captured. | Back Office default |
| **Card types** | Limit the card brands shown on the payment page. Leave empty to use all card types configured in your Back Office. See the list of supported card types below. | (all) |

#### Supported card types

When you restrict card types, you can choose from the following options. Select none to use the full set configured in your Sogecommerce Back Office.

| Code | Label |
|------|-------|
| CB | CB |
| E-CARTEBLEUE | e-Carte Bleue |
| MAESTRO | Maestro |
| MASTERCARD | MasterCard |
| VISA | Visa |
| VISA_ELECTRON | Visa Electron |
| VPAY | V PAY |
| AMEX | American Express |
| AURORE_MULTI | Carte Aurore |
| ILLICADO | Carte Cadeau Illicado |
| ONEY | FacilyPay Oney |
| MASTERPASS | MasterPass |
| PAYPAL | PayPal |
| SDD | Prélèvement SEPA Direct Debit |
| PAYLIB | Wallet Paylib |

---

### Selective 3DS

The selective 3DS feature lets you disable 3D Secure for low-value orders, reducing friction for small purchases while keeping protection on higher-value ones.

| Field | Description | Default |
|-------|-------------|---------|
| **Disable 3DS below** | Orders below this amount will skip 3DS verification. Leave empty to always apply 3DS. Requires a subscription to the selective-3DS option from Sogecommerce. | (disabled) |

---

### Amount restrictions

| Field | Description | Default |
|-------|-------------|---------|
| **Minimum amount** | The payment method is hidden from the checkout if the order total is below this value. Leave empty for no minimum. | (none) |
| **Maximum amount** | The payment method is hidden from the checkout if the order total exceeds this value. Leave empty for no maximum. | (none) |

---

### Return to shop

These settings control what happens on the Sogecommerce payment page after payment completes or fails, before the buyer is sent back to your store.

| Field | Description | Default |
|-------|-------------|---------|
| **Automatic redirection** | When enabled, the payment page automatically redirects the buyer back to your store after the timeout expires. When disabled, the buyer must click a button to return. | No |
| **Redirection timeout on success** | Seconds (0–300) before the buyer is redirected after a successful payment. Only applies when automatic redirection is on. | 5 |
| **Redirection message on success** | Message shown on the payment page during the countdown after a successful payment. | "Redirection to shop in a few seconds..." |
| **Redirection timeout on failure** | Seconds (0–300) before the buyer is redirected after a declined payment. Only applies when automatic redirection is on. | 5 |
| **Redirection message on failure** | Message shown on the payment page during the countdown after a declined payment. | "Redirection to shop in a few seconds..." |
| **Return mode** | The HTTP method Sogecommerce uses when sending the buyer back to your store. **GET** appends result parameters to the URL. **POST** sends them in the request body. GET is suitable for most setups. | GET |

---

### Order status mapping

These settings control which J2Commerce order status is applied when Sogecommerce reports each payment outcome. You can map each outcome to any order status you have defined under **J2Commerce** -> **Localisation** -> **Order Statuses**.

<!-- SCREENSHOT: Order status mapping section showing four dropdowns for Confirmed, Pending, Cancelled, and Failed payment status -->

| Field | Description | Default status |
|-------|-------------|----------------|
| **Confirmed payment status** | Applied when the payment is authorised and accepted (gateway statuses: AUTHORISED, CAPTURED, ACCEPTED, and related). | Confirmed (1) |
| **Pending payment status** | Applied when the payment is awaiting confirmation (gateway statuses: WAITING_AUTHORISATION, UNDER_VERIFICATION, and related). | Pending (4) |
| **Cancelled payment status** | Applied when the buyer abandons the payment page without paying (gateway statuses: ABANDONED, NOT_CREATED). | Cancelled (6) |
| **Failed payment status** | Applied when the payment is refused or encounters an error. | Failed (3) |

When a payment is confirmed, J2Commerce sends the customer a confirmation email. For pending, cancelled, and failed outcomes, no customer notification is sent by default.

---

### Payment surcharge (optional)

You can optionally add a surcharge to cover processing fees. The surcharge is added to the order before checkout completes.

| Field | Description |
|-------|-------------|
| **Surcharge name** | Label shown to the customer on the checkout and order summary (for example, "Payment processing fee"). |
| **Surcharge percentage** | Percentage of the order subtotal plus shipping to add as a fee. For example, enter `1.5` for 1.5%. |
| **Surcharge fixed amount** | Fixed amount to add as a fee in the store's base currency. You can combine this with a percentage. |
| **Surcharge tax class** | Optional tax profile to apply to the surcharge amount. Leave empty for no tax on the surcharge. |

---

### Geozone restriction (optional)

| Field | Description |
|-------|-------------|
| **Geozone restriction** | Show this payment method only to buyers whose billing address falls within the selected geozone. Leave empty to show to all customers. Geozones are configured under **J2Commerce** -> **Localisation** -> **Geozones**. |

---

### Additional options

| Field | Description | Default |
|-------|-------------|---------|
| **Thank-you article** | A Joomla article to display on the order confirmation page after payment. Optional. | (none) |
| **On selection text** | HTML or text shown when the buyer selects this payment method at checkout. | (none) |
| **On before payment text** | HTML or text shown on the pre-payment page before the buyer is redirected. | (none) |
| **On after payment text** | HTML or text shown on the post-payment page after the buyer returns. | (none) |
| **Button text** | Label on the "Place Order" button. | "Place Order" |
| **Debug mode** | Logs all gateway requests and responses to the Joomla log directory. Disable in production — logs may contain sensitive order data. | No |
| **Subtemplate** | Choose the layout framework (Bootstrap 5 or UIkit) for the pre-payment page. Leave on **Auto** to match the active storefront template. | Auto |

---

## Supported currencies

Sogecommerce accepts a wide range of currencies. The plugin automatically checks that the store's active currency is supported before showing this payment method. If the currency is not supported, the payment option is hidden.

Supported currencies include EUR, GBP, USD, CAD, AUD, CHF, JPY, SEK, NOK, DKK, PLN, CZK, HUF, HKD, SGD, BRL, MXN, INR, and many others.

---

## Testing your setup

1. Set **Mode** to **TEST**.
2. Use the default test credentials (the Back Office provides test card numbers for each card type).
3. Place a test order in your store and confirm you are redirected to the Sogecommerce test payment page.
4. Complete the test payment and verify the order status updates correctly in **J2Commerce** -> **Sales** -> **Orders**.
5. Check that you received a test confirmation email if the order was mapped to a notifying status.
6. Once confirmed, set **Mode** to **PRODUCTION** and enter your production key.

---

## Troubleshooting

### Signature mismatch — payment page rejects the request

**Cause:** The **Signature algorithm** set in the plugin does not match the algorithm configured in the Sogecommerce Back Office.

**Solution:**
1. Log into the Sogecommerce Back Office.
2. Go to **Settings** -> **Technical configuration** (or your account's equivalent section).
3. Note the hash/signature algorithm configured there.
4. In the J2Commerce plugin, set **Signature algorithm** to the same value.
5. Save the plugin and try a test payment again.

If you recently changed the algorithm in the Back Office without updating the plugin, this is the most likely cause of a failed redirect.

### Orders are not updated after payment

**Cause:** The IPN (server-to-server notification) URL has not been registered in the Sogecommerce Back Office, or the URL is incorrect.

**Solution:**
1. In the plugin settings, find the **Instant Payment Notification (IPN) URL** note field — it shows the URL your site expects.
2. Log into the Sogecommerce Back Office and go to **Settings** -> **Notification rules**.
3. Open the instant payment notification rule and paste the correct URL into both the test and production notification URL fields.
4. Save and re-test.

Also check: if your Joomla site is in **Offline** mode or a maintenance plugin is blocking anonymous requests, the IPN call from Sogecommerce's servers will be rejected before it reaches J2Commerce.

### Payment method not shown at checkout

**Possible causes:**

- The store's active currency is not supported by Sogecommerce. Check the supported currencies list above.
- The order total is below the **Minimum amount** or above the **Maximum amount** configured in the plugin.
- A **Geozone restriction** is set and the buyer's billing address is not within that geozone.
- The plugin is disabled. Go to **System** -> **Plugins**, search for "Sogecommerce", and confirm it is enabled.

### Debug logging

Enable **Debug mode** in the plugin settings to log all gateway requests and responses. Logs are written to the Joomla log directory (`/logs/` by default) in a file named `plg_j2commerce_payment_sogecommerce`. Review the log to see exactly what is being sent to Sogecommerce and what response is returned.

Disable debug mode once the issue is resolved. Debug logs may contain order details.

---

## Related topics

- [Order Statuses](../localisation/order-statuses.md)
- [Geozones](../localisation/geozones.md)
- [Currencies](../localisation/currencies.md)
- [Payment Methods overview](../setup/index.md)
