---
title: "PayU LATAM"
sidebar_label: "PayU LATAM"
sidebar_position: 30
description: "Accept payments across Latin America via the PayU LATAM Web Checkout hosted payment page. Supports Argentina, Brazil, Chile, Colombia, Mexico, and Peru."
---

# PayU LATAM

PayU LATAM is a hosted payment gateway that lets your customers pay using local payment methods across Latin America. When a customer chooses PayU at checkout, they are redirected to PayU's secure payment page — your server never handles card data, which keeps you in the simplest PCI compliance tier (SAQ A). After the transaction, PayU sends a signed server-to-server notification back to your store to update the order automatically.

## Supported Countries and Currencies

PayU LATAM supports merchants in the following countries. Each country uses its own Account ID in the PayU dashboard.

| Country | Settlement Currency | Currency Code |
|---------|-------------------|---------------|
| Argentina | Argentine Peso | ARS |
| Brazil | Brazilian Real | BRL |
| Chile | Chilean Peso | CLP |
| Colombia | Colombian Peso | COP |
| Mexico | Mexican Peso | MXN |
| Peru | Peruvian Sol | PEN |
| — | US Dollar | USD |

**Currency conversion:** If your J2Commerce store operates in a currency other than the six LATAM currencies listed above, the plugin automatically converts the order total to USD for submission to PayU. Make sure you have a USD currency rate configured under **J2Commerce** -> **Configuration** -> **Currencies** so the conversion is accurate.

## Prerequisites

- J2Commerce installed and active
- A PayU LATAM merchant account — sign up at [https://www.payulatam.com](https://www.payulatam.com)
- Your **Merchant ID**, **Account ID**, and **API Key** from the PayU dashboard

## Installation

This plugin is a separate add-on available from the [J2Commerce Extensions Store](https://www.j2commerce.com). It is not included with the core J2Commerce component.

1. Purchase and download the `payment_payulatam.zip` package from the J2Commerce website.
2. Go to **System** -> **Install** -> **Extensions**.
3. Upload the `payment_payulatam.zip` package file.
4. The plugin installs and enables automatically.

After installation, go to **J2Commerce** -> **Payments** -> **Payment Methods** to configure it.

<!-- SCREENSHOT: J2Commerce Payments > Payment Methods list showing PayU LATAM entry -->

## Finding Your Credentials in the PayU Dashboard

You need three credentials from your PayU merchant account. Log in at [https://merchants.payulatam.com](https://merchants.payulatam.com).

**Merchant ID**
Go to **Settings** -> **Technical Configuration**. The **Merchant ID** appears at the top of the page. This value is the same across all countries under your account.

**Account ID**
Each country you sell in has a separate Account ID. In the left sidebar, select the country (e.g., Colombia), then go to **Settings** -> **Technical Configuration**. The **Account ID** for that country appears there. Use the Account ID that matches the primary market you are targeting.

**API Key**
On the same **Technical Configuration** page, scroll to the **API Key** section. This is a secret value — treat it like a password. Do not share it or store it in source control.

<!-- SCREENSHOT: PayU dashboard Technical Configuration page with Merchant ID, Account ID, and API Key highlighted -->

## Configuration

Open the plugin settings at **J2Commerce** -> **Payments** -> **Payment Methods**, then click **PayU LATAM**.

### Credentials

| Field | Description | Recommended Value |
|-------|-------------|-------------------|
| **Display Name** | The name shown to customers at checkout | `PayU LATAM` |
| **Display Image** | Optional logo shown next to the payment option | PayU logo file |
| **Merchant ID** | Your PayU Merchant ID (live account) | From PayU dashboard |
| **Account ID** | Country-specific Account ID (live account) | From PayU dashboard |
| **API Key** | Your PayU API Key (live account) | From PayU dashboard |

### Test Mode

| Field | Description | Recommended Value |
|-------|-------------|-------------------|
| **Sandbox Mode** | Routes transactions through PayU's test environment | **No** for live; **Yes** for testing |
| **Sandbox Merchant ID** | Merchant ID for your PayU test account | From PayU sandbox dashboard |
| **Sandbox Account ID** | Account ID for your PayU test account | From PayU sandbox dashboard |
| **Sandbox API Key** | API Key for your PayU test account | From PayU sandbox dashboard |

The sandbox fields are hidden until you enable **Sandbox Mode**. When sandbox is on, the plugin sends all transactions to `sandbox.checkout.payulatam.com` using your sandbox credentials — your live credentials are not used at all.

### Security

| Field | Description | Recommended Value |
|-------|-------------|-------------------|
| **Hash Algorithm** | Signing algorithm for request and response signatures | Must match what is configured in your PayU account |
| **Validate Response Signature** | Verifies the PayU confirmation signature before updating the order | **Yes** (strongly recommended) |

**Important:** The Hash Algorithm must match exactly what your PayU account is configured to use. If these differ, every confirmation from PayU will fail the signature check and orders will not update. The default is MD5. Log in to your PayU dashboard under **Settings** -> **Technical Configuration** to confirm which algorithm is active.

### Payment Page Language

| Field | Description | Options |
|-------|-------------|---------|
| **Payment Page Language** | Language displayed on the PayU hosted payment page | English, Spanish, Portuguese |

Choose the language that matches your customers. Spanish is the most widely used across LATAM markets.

### Order Statuses

| Field | Description | Default |
|-------|-------------|---------|
| **Confirmed Payment Status** | Status applied when PayU approves the payment | Confirmed (1) |
| **Pending Payment Status** | Status applied when the payment is processing | Pending (4) |
| **Failed Payment Status** | Status applied when the payment is declined or expired | Failed (3) |

These map to your store's order statuses configured in **J2Commerce** -> **Configuration** -> **Order Statuses**. The defaults work for most stores.

### Surcharge

You can add an optional fee for customers who pay with PayU. Leave both fields blank if you do not want to add a fee.

| Field | Description | Example |
|-------|-------------|---------|
| **Surcharge Name** | Label shown on the order for the fee | `PayU processing fee` |
| **Surcharge (%)** | Percentage of the order subtotal | `2.5` |
| **Surcharge (Fixed)** | Fixed amount added to the order | `1.00` |
| **Surcharge Tax Class** | Tax profile applied to the surcharge | Select if the fee is taxable |

### Geozone Restriction

The **Geozone Restriction** field limits PayU to customers in specific countries or regions. Leave it blank to make PayU available to all customers. To restrict it — for example, to show PayU only to LATAM customers — select a geozone that contains those countries. You manage geozones under **J2Commerce** -> **Configuration** -> **Geo Zones**.

### Thank-You Article

The **Thank-You Article** field lets you link a Joomla article that is displayed on the confirmation page after a successful payment. This is a good place to add order instructions, download links, or a welcome message.

### Customer Messages

| Field | Description |
|-------|-------------|
| **On Selection Text** | Message shown when the customer selects PayU at checkout |
| **Before Payment Text** | Message shown on the review step, just before the customer is redirected to PayU |
| **After Payment Text** | Message shown on the confirmation page after returning from PayU |
| **On Cancel Text** | Message shown if the customer clicks Cancel on the PayU page |
| **On Error Text** | Message shown if an unexpected error occurs during processing |

All message fields accept plain text or basic HTML. The **On Cancel** and **On Error** messages have sensible defaults and can be left blank unless you want to customize them.

### Button Text

The **Button Text** field sets the label on the checkout button that redirects the customer to the PayU payment page. The default is "Place Order".

### Admin Email Group

The **Admin Email Group** field selects which Joomla user group receives an email notification when a PayU confirmation fails signature validation. The default is group 8 (Super Users). This alert helps you investigate problems quickly — for example, if someone tampers with a callback URL or if a configuration mismatch causes signature errors.

### Debug Mode

The **Debug Mode** toggle writes detailed information to Joomla's log system, including the signature input strings and the full PayU confirmation payload. The log category is `plg_j2commerce_payment_payulatam`. Enable this only when diagnosing a problem — leave it off in production.

Logs are accessible at **System** -> **Maintenance** -> **Joomla Logs** (or directly in the `logs/` directory of your Joomla installation).

### Subtemplate

The **Subtemplate** field controls which frontend template the plugin uses for its payment form. Leave it on **Auto** to inherit the template from your active J2Commerce storefront (Bootstrap 5 or UIkit). Change it only if you are using a custom layout override.

<!-- SCREENSHOT: PayU LATAM plugin settings page with all fields visible -->

## IMPORTANT: Configure the Confirmation URL in Your PayU Dashboard

PayU sends a server-to-server HTTP POST to your store after each transaction. This is how orders get updated automatically — it does not depend on the customer returning to your site. **You must register this URL in your PayU account or confirmed orders will not update.**

The plugin generates two callback URLs automatically. You do not type these in the plugin settings — they are built from your site's URL. The URLs follow this pattern:

- **Response URL** (browser redirect after payment): `https://yoursite.com/index.php?option=com_j2commerce&task=checkout.confirmPayment&orderpayment_type=payment_payulatam&paction=display`
- **Confirmation URL** (server-to-server IPN): `https://yoursite.com/index.php?option=com_j2commerce&task=checkout.confirmPayment&orderpayment_type=payment_payulatam&paction=process`

To register the Confirmation URL in PayU:

1. Log in to [https://merchants.payulatam.com](https://merchants.payulatam.com).
2. Go to **Settings** -> **Technical Configuration**.
3. Paste your Confirmation URL into the **Confirmation URL** field.
4. Paste your Response URL into the **Response URL** field.
5. Click **Save**.

<!-- SCREENSHOT: PayU Technical Configuration page showing Confirmation URL and Response URL fields -->

PayU retries the confirmation several times over several hours if your server does not respond with a `200 OK`. The plugin handles duplicate confirmations safely — if an order is already marked as approved, a second confirmation from PayU is ignored.

## Testing with Sandbox

1. Enable **Sandbox Mode** in the plugin settings.
2. Enter your sandbox credentials (Sandbox Merchant ID, Sandbox Account ID, Sandbox API Key).
3. Log in to the PayU sandbox dashboard and register your Confirmation URL pointing to your development site.
4. Place a test order in your store and go through checkout.
5. You will be redirected to `sandbox.checkout.payulatam.com`.
6. Use the test card details provided in the PayU developer documentation to simulate approved, pending, and declined transactions.
7. Return to J2Commerce and verify the order status updated correctly.

When you are ready to go live, turn **Sandbox Mode** off. The plugin will switch to your live credentials and the live PayU gateway automatically.

## Troubleshooting

### Orders are not updating after payment

**Cause:** PayU cannot reach your Confirmation URL.

**Solution:**

1. Check that your Confirmation URL is correctly registered in the PayU dashboard under **Settings** -> **Technical Configuration**.
2. Verify the URL is publicly accessible — it must be reachable from PayU's servers. A URL on `localhost` or behind a VPN will not work.
3. Check your server's firewall rules. PayU's confirmation POST must be able to reach your store over HTTPS on port 443.
4. Enable **Debug Mode** in the plugin and check the Joomla logs for any incoming confirmation records.

### Signature mismatch errors

**Cause:** The **Hash Algorithm** in the plugin does not match the algorithm configured in your PayU account.

**Solution:**

1. Log in to your PayU dashboard and go to **Settings** -> **Technical Configuration**.
2. Note which hash algorithm is shown there (MD5, SHA-1, or SHA-256).
3. Open the plugin settings and set **Hash Algorithm** to the same value.
4. Save the plugin settings.
5. If you are also receiving admin notification emails about failed signatures, check that the **API Key** entered in the plugin matches the one in your PayU dashboard exactly — a trailing space or copied character can cause every signature to fail.

### Order is stuck on Pending

**Cause:** PayU returned a `state_pol` value of `7` (pending), which means the payment is still being processed by PayU — this is normal for some local payment methods like cash vouchers or bank transfers.

**Solution:** Wait for PayU to process the payment. PayU will send a follow-up confirmation once the status resolves. The order will update automatically when that confirmation arrives. If the order remains pending after 48 hours, log in to your PayU dashboard and check the transaction status there directly.

### Currency conversion is incorrect

**Cause:** Your store currency is not one of the six supported LATAM currencies (ARS, BRL, CLP, COP, MXN, PEN), so the plugin falls back to USD. The USD exchange rate in your J2Commerce currency table may be out of date or missing.

**Solution:**

1. Go to **J2Commerce** -> **Configuration** -> **Currencies**.
2. Confirm that USD is listed and that its exchange rate is current.
3. If you use the Currency Updater app, run an update to refresh the rates.

### Confirmation URL is not being called during testing

**Cause:** When testing on a local development environment, PayU's servers cannot reach a URL on `localhost` or `127.0.0.1`.

**Solution:** Use a tunneling tool such as ngrok to expose your local site over a public HTTPS URL, and register that URL in the PayU sandbox dashboard as the Confirmation URL during development.

## What's New in J2Commerce vs the Old J2Store Version

| Feature | J2Store Version | J2Commerce Version |
|---------|----------------|-------------------|
| Response signature verification | Not present — confirmations were accepted without verifying the PayU signature | Verified using `hash_equals()` — timing-safe comparison prevents forgery |
| Order status mapping | Hardcoded status IDs for approved/pending/failed | Configurable via dropdown — map to any order status in your store |
| Admin failure notification | Not present | Email sent to a configurable user group when signature validation fails |
| Debug logging | PHP error log | Joomla's structured log system with category filter |
| Frontend templates | Single template | Dual templates — Bootstrap 5 and UIkit, with auto-detection |
| Double-charge protection | Not present — retried confirmations could update an already-confirmed order multiple times | Built-in guard ignores duplicate confirmations for already-approved orders |
| Sandbox credentials | Toggled by modifying the live credential fields | Separate sandbox fields shown only when sandbox mode is enabled |
