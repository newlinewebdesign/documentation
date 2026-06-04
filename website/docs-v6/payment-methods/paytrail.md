---
title: "Paytrail Payment"
sidebar_label: "Paytrail"
sidebar_position: 30
description: "Accept Finnish bank transfers, mobile wallets, and buy-now-pay-later via Paytrail. EUR-only payment aggregator for Finnish and Nordic merchants."
---

# Paytrail Payment

Paytrail is a Finnish payment aggregator used by thousands of online stores in Finland and the Nordic region. Instead of entering card details, shoppers choose from a grid of familiar Finnish payment providers — OP, Nordea, S-Pankki, Pivo, MobilePay, Klarna, and more — and complete payment through their own bank or wallet.

This plugin is ideal for stores selling to Finnish customers. Because Paytrail settles transactions in euros, **your J2Commerce store currency must be set to EUR**. Orders placed in any other currency are rejected at the payment step.

This plugin is a separate add-on available from the [J2Commerce Extensions Store](https://www.j2commerce.com). It is not included with the core J2Commerce 6 component.

---

## Prerequisites

Before you start, make sure you have:

- J2Commerce installed and active on your Joomla 6 site
- A Paytrail merchant account (sign up at the Paytrail website)
- Your **Merchant ID** and **Merchant Secret** from the Paytrail merchant dashboard
- Your store currency set to **EUR** in J2Commerce

---

## Step 1: Get Your Paytrail Credentials

Paytrail issues two credentials to every merchant:

- **Merchant ID** — a short numeric identifier for your account
- **Merchant Secret** — a longer secret key used to sign and verify payments

To find these credentials:

1. Log in to your Paytrail merchant dashboard.
2. Navigate to the **Settings** or **Integration** section (the exact label depends on your dashboard version).
3. Copy the **Merchant ID** and **Merchant Secret** and store them somewhere safe.

<!-- SCREENSHOT: Paytrail merchant dashboard showing Merchant ID and Merchant Secret fields -->

Keep your Merchant Secret private. Anyone who has it can verify or forge payment callbacks for your account.

---

## Step 2: Install the Plugin

1. Purchase and download the `plg_j2commerce_payment_paytrail.zip` package from the [J2Commerce website](https://www.j2commerce.com).
2. In the Joomla admin panel, go to **System** -> **Install** -> **Extensions**.
3. Upload the zip file using the **Upload Package File** area.
4. Joomla installs the plugin automatically.

<!-- SCREENSHOT: Joomla Extension Manager upload screen with the Paytrail zip selected -->

---

## Step 3: Enable the Plugin

After installation the plugin is disabled by default.

**Option A:** Go to **J2Commerce** -> **Setup** -> **Payment Methods**. Find **Paytrail** in the list and click the status toggle to enable it.

**Option B:** Go to **System** -> **Manage** -> **Extensions**, search for **Paytrail**, and click **Enable**.

Once enabled, click the **Paytrail** title to open the configuration screen.

<!-- SCREENSHOT: J2Commerce Payment Methods list with Paytrail highlighted -->

---

## Step 4: Configure the Plugin

The configuration screen is divided into three fieldsets: **Basic Settings**, **Paytrail Settings**, and **Advanced**.

### Basic Settings

| Field | What it does | Recommended value |
|-------|-------------|-------------------|
| **Display Name** | The label shown to shoppers at checkout when they choose a payment method | `Paytrail` or `Online banking` |
| **Display Image** | Optional logo shown next to the payment method name at checkout | Upload your preferred image or leave empty |
| **Geo-Zone Restriction** | Limit Paytrail to shoppers in a specific geo-zone (for example, Finland only) | Select a Finland or EU geo-zone, or leave empty to show Paytrail to everyone |
| **Thank You Article** | A Joomla article displayed on the confirmation page after a successful payment | Optional — choose an article if you want to show custom post-purchase content |

### Paytrail Settings

| Field | What it does | Recommended value |
|-------|-------------|-------------------|
| **Merchant ID** | Your numeric Merchant ID from the Paytrail dashboard | Enter the value from your dashboard |
| **Merchant Secret** | Your secret key for signing payments | Enter the value from your dashboard |
| **Language** | Language code sent to Paytrail for the payment provider selection page | `FI` for Finnish, `EN` for English, `SV` for Swedish |
| **Payment selection page info text** | Introductory text shown above the provider buttons on the payment page | Optional — for example: "Select your preferred payment method." |

<!-- SCREENSHOT: Paytrail Settings fieldset showing Merchant ID, Merchant Secret, Language, and Info text fields -->

### Advanced

| Field | What it does | Recommended value |
|-------|-------------|-------------------|
| **Order Status on Success** | The J2Commerce order status applied when Paytrail confirms payment | Choose **Confirmed** or your equivalent paid status |
| **Order Status on Failure** | The status applied when payment fails or is rejected | Choose **Failed** or **Pending Payment** |
| **Surcharge Percent** | Add a percentage of the order subtotal as a payment fee | Leave empty for no surcharge |
| **Surcharge Fixed** | Add a fixed euro amount as a payment fee | Leave empty for no surcharge |
| **Debug Mode** | Write detailed payment activity to `administrator/logs/payment_paytrail.php` | **No** in production; **Yes** only when troubleshooting |

Click **Save** when you are done.

---

## Step 5: Set Your Store Currency to EUR

Paytrail only accepts EUR payments. The plugin checks the order currency and displays a clear error message if it is not EUR.

To verify your store currency:

1. Go to **J2Commerce** -> **Configuration** -> **General**.
2. Confirm the default currency is set to **Euro (EUR)**.
3. If you use a multi-currency app, make sure EUR is the base or transaction currency.

---

## How Checkout Looks to Your Shoppers

When a shopper selects Paytrail at checkout, the payment page shows a grid of Finnish payment provider buttons — grouped by type:

- **Bank transfers** — OP, Nordea, Aktia, S-Pankki, Handelsbanken, Ålandsbanken, and others
- **Mobile wallets** — Pivo, MobilePay, Siirto
- **Buy now, pay later** — Klarna, and any other BNPL providers enabled on your Paytrail account

<!-- SCREENSHOT: Paytrail payment provider grid showing bank buttons grouped by type -->

The shopper clicks their preferred provider button. They are redirected to that provider's own login or payment page. After completing payment there, they are automatically returned to your store's confirmation page.

The complete flow:

1. Shopper selects Paytrail at checkout and clicks **Place Order**.
2. A grid of payment provider buttons appears.
3. Shopper clicks their bank or wallet button.
4. Shopper is redirected to the provider and completes payment.
5. Provider redirects the shopper back to your store.
6. J2Commerce verifies the return, updates the order status to your configured paid status, and shows the confirmation page.

Paytrail also sends a server-to-server callback as a safety net. If the shopper closes their browser before returning, the order is still marked paid when Paytrail's callback arrives.

---

## Testing

Paytrail provides published sandbox credentials for testing. These are Paytrail's well-known public test values:

| Credential | Value |
|-----------|-------|
| **Merchant ID** | `375917` |
| **Merchant Secret** | `SAIPPUAKAUPPIAS` |

To test a payment:

1. Open the plugin settings and enter the sandbox Merchant ID (`375917`) and Merchant Secret (`SAIPPUAKAUPPIAS`).
2. Set **Debug Mode** to **Yes**.
3. Place a test order through your store in EUR. Add any product to the cart and proceed to checkout.
4. At the payment step, select Paytrail.
5. Click any provider button — in the sandbox the buttons redirect to a Paytrail test page where you can simulate a successful or failed payment.
6. After simulating payment, return to your store and confirm the order status changed to your configured **Order Status on Success**.

<!-- SCREENSHOT: Paytrail sandbox provider grid in test mode -->

Once testing is complete:

1. Replace the sandbox credentials with your live Merchant ID and Merchant Secret.
2. Set **Debug Mode** back to **No**.
3. Place a real small-value order to verify the live integration works end-to-end.

---

## What Changed from the J2Store Version

If you previously used the J2Store Paytrail plugin, here is what is new in the J2Commerce 6 version:

- **Native Joomla 6 architecture** — no FOF 2 dependency; runs cleanly on Joomla 6 with full DI container and event subscriber support.
- **Secure HMAC validation** — payment return and callback responses are validated against the Paytrail HMAC signature using the official Paytrail PHP SDK. The old plugin passed raw superglobal data to the validator; the new plugin uses Joomla's filtered input layer.
- **EUR guard** — orders in non-EUR currencies are rejected immediately at the payment step with a clear message, instead of silently failing at the Paytrail API.
- **Geozone enforcement** — the plugin now actively hides from shoppers whose billing address is outside the configured geo-zone, not just suppresses it in the list.
- **Surcharge support** — you can add a percentage or fixed surcharge to Paytrail orders (parity with PayPal and bank transfer).
- **Order status fields** — dedicated fields let you choose the exact order status set on success and on failure, rather than using hardcoded values.
- **Debug logging** — a toggleable debug mode writes detailed payment activity to `administrator/logs/payment_paytrail.php` so you can diagnose issues without hunting through Joomla's system log.
- **Idempotency guard** — if Paytrail's browser return and server callback both arrive, the order is only finalized once; duplicate finalization is safely skipped.
- **Vanilla JS** — no jQuery dependency on the checkout page.

---

## Troubleshooting

### No payment provider buttons appear at checkout

**Cause:** The Merchant ID or Merchant Secret is wrong, or the store currency is not EUR.

**Solution:**

1. Open the Paytrail plugin settings and double-check the **Merchant ID** and **Merchant Secret** match what is shown in your Paytrail dashboard exactly (no extra spaces).
2. Confirm your store currency is set to EUR.
3. Enable **Debug Mode** and attempt checkout again. Check `administrator/logs/payment_paytrail.php` for any error messages from the Paytrail API.

### "Paytrail only accepts EUR payments" message

**Cause:** The order currency is not EUR.

**Solution:** Set your J2Commerce store currency to EUR. If you use a multi-currency app, ensure EUR is the transaction currency when Paytrail is selected.

### "HMAC signature validation failed" error

**Cause:** The **Merchant Secret** is incorrect, or the return URL was modified in transit by a proxy or caching layer.

**Solution:**

1. Verify the **Merchant Secret** in the plugin matches your Paytrail dashboard exactly (it is case-sensitive).
2. If you use a proxy, CDN, or caching plugin, make sure it does not rewrite or cache the Paytrail callback URL (`confirmPayment`).
3. Enable **Debug Mode** and check the log for details about which parameters failed validation.

### Order not marked as paid after the shopper returns

**Cause:** The return URL callback was blocked by a firewall, caching layer, or the shopper closed the browser before the redirect completed.

**Solution:**

1. Check whether Paytrail's server-to-server callback URL is accessible from the internet. The callback URL is your site's checkout `confirmPayment` endpoint with `paction=callback`.
2. Make sure your hosting firewall or security plugin does not block POST requests to that URL.
3. Enable **Debug Mode** and check `administrator/logs/payment_paytrail.php` for callback entries. An entry starting with `postPayment paction=callback` confirms Paytrail reached your site.

### Paytrail is not showing at checkout even though it is enabled

**Cause:** The **Geo-Zone Restriction** setting is excluding the shopper's billing country.

**Solution:**

1. Open the plugin settings and check the **Geo-Zone Restriction** field.
2. If it is set to a specific geo-zone, confirm the shopper's billing country is included in that geo-zone under **J2Commerce** -> **Localization** -> **Geo-Zones**.
3. To show Paytrail to all shoppers regardless of location, clear the **Geo-Zone Restriction** field and save.

### Debug log not appearing

**Cause:** **Debug Mode** is set to **No**, or the `administrator/logs/` directory is not writable.

**Solution:**

1. Set **Debug Mode** to **Yes** in the plugin settings and save.
2. Attempt a checkout again.
3. Check that Joomla's log path (`administrator/logs/`) exists and is writable by the web server.

---

## Support

- **J2Commerce documentation:** [docs.j2commerce.com](https://docs.j2commerce.com)
- **J2Commerce support and extensions:** [www.j2commerce.com](https://www.j2commerce.com)
- **Paytrail merchant documentation:** available from your Paytrail merchant dashboard
