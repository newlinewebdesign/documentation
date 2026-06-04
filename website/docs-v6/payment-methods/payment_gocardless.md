---
title: "GoCardless Direct Debit"
sidebar_label: "GoCardless Direct Debit"
sidebar_position: 46
description: "Accept bank Direct Debit payments via GoCardless — Bacs, SEPA, ACH, BECS, and PAD — with async payment confirmation, webhook-driven order updates, and admin refund tools."
---

# GoCardless Direct Debit

The GoCardless Direct Debit plugin connects your J2Commerce store to [GoCardless](https://gocardless.com), a specialist in bank-to-bank payments. Instead of entering a credit card, the customer authorises a Direct Debit mandate — a permission that lets you collect money directly from their bank account. GoCardless supports multiple schemes depending on the customer's country:

- **Bacs** — UK bank accounts
- **SEPA Core** — Eurozone bank accounts
- **ACH** — US bank accounts
- **BECS** — Australian bank accounts
- **PAD** — Canadian bank accounts

GoCardless is a strong choice for stores with subscription products, membership fees, or regular recurring orders because the stored mandate can be reused for future payments without the customer going through checkout again.

:::note

GoCardless is **not** a card processor. Customers pay from their bank account, not with a credit or debit card. Payments take 2–5 business days to clear — they do not confirm instantly. This is normal for bank debit and is explained clearly to customers during checkout.

:::

## Requirements

- Joomla 6.x
- J2Commerce 6.x
- PHP 8.3 or later
- A GoCardless merchant account (free to create at [gocardless.com](https://gocardless.com))
- Your store must be publicly accessible over HTTPS — webhooks cannot reach a local or private server

## Purchase and Download

This plugin is a separate add-on available from the [J2Commerce Extensions Store](https://www.j2commerce.com). It is not included with the core J2Commerce 6 component.

1. Go to the [J2Commerce website](https://www.j2commerce.com) and locate **GoCardless Direct Debit**.
2. Add it to your cart and complete checkout.
3. Go to **My Downloads** under your account profile and find the plugin.
4. Click **Available Versions** -> **View Files** -> **Download Now** to download the ZIP file.

## Get Your GoCardless Credentials

Before configuring the plugin you need three pieces of information from your GoCardless account.

### Sandbox (testing)

1. Log in to the [GoCardless sandbox dashboard](https://manage-sandbox.gocardless.com).
2. Go to **Developers** -> **Create access token** and generate a token with read/write permissions. It starts with `sandbox_`.
3. Note your **Creditor ID** from **Settings** -> **Creditor details**. It starts with `CR`.
4. To receive webhooks during testing, go to **Developers** -> **Webhooks** -> **Create endpoint**, enter your webhook URL (see [Setting Up Webhooks](#setting-up-webhooks) below), and copy the **Signing secret**.

### Live account

1. Log in to your live [GoCardless dashboard](https://manage.gocardless.com).
2. Go to **Developers** -> **Create access token**. Your live token starts with `live_`.
3. Find your live **Creditor ID** under **Settings** -> **Creditor details**.
4. Register a live webhook endpoint and copy its signing secret.

Keep these values at hand while configuring the plugin.

## Install the Plugin

In the Joomla Administrator, go to **System** -> **Install** -> **Extensions**.

Upload the `payment_gocardless.zip` file or use the Install from URL option.

![](/img/install.webp)

## Enable the Plugin

Once installed, you need to enable the plugin. There are two ways to reach the Payment Methods list.

**Option A:** Click the **J2Commerce** icon at the top right corner -> **Setup** -> **Payment Methods**

**Option B:** Go to **Components** on the left sidebar -> **J2Commerce** -> **Dashboard** -> **Setup** -> **Payment Methods**

![](/img/culqi.webp)

Search for **GoCardless Direct Debit**, click the **X**, and it will turn into a green checkmark. The plugin is now enabled and ready to configure.

## Configure the Plugin

Click the **GoCardless Direct Debit** title to open its settings.

:::tip

Click the **Toggle Inline Help** button at the top of any plugin settings screen to show a description below each field.

:::

### Appearance

**Display Name:** The label shown to customers on the checkout page. Default is `GoCardless Direct Debit` — you can change this to something like "Pay by bank transfer" if that wording suits your audience.

**Display Image:** Optional logo to show beside the payment method name at checkout.

### Sandbox / Test Mode

**Sandbox / Test Mode:** When enabled, all transactions go to the GoCardless sandbox environment — no real money moves. Default is **Yes** (sandbox on).

Enable this during initial setup and testing. Switch it to **No** when you are ready to accept real payments. When sandbox mode is active, a warning appears on the J2Commerce dashboard.

### Live Credentials

These fields appear when **Sandbox / Test Mode** is turned **off**.

**Live Access Token:** Your GoCardless live access token (starts with `live_`). Keep this value secret — treat it like a password.

**Live Webhook Endpoint Secret:** The signing secret from your live GoCardless webhook endpoint. The plugin uses this to verify that incoming webhook notifications genuinely came from GoCardless.

**Live Creditor ID:** Your GoCardless creditor ID (starts with `CR`). This is used to check whether your account has refunds enabled.

### Sandbox Credentials

These fields appear when **Sandbox / Test Mode** is turned **on**.

**Sandbox Access Token:** Your GoCardless sandbox access token (starts with `sandbox_`). A test token is pre-filled by default so you can try the flow immediately.

**Sandbox Webhook Endpoint Secret:** The signing secret for your sandbox webhook endpoint.

**Sandbox Creditor ID:** Your GoCardless sandbox creditor ID. Pre-filled with a J2Commerce test creditor by default.

### Mandate Renewals

**Allow Mandate Renewals:** When enabled, a stored mandate from a previous order can be used to collect subscription renewal payments automatically, without the customer going through checkout again. Default is **Yes**.

Turn this off only if you do not use the Subscriptions add-on or if you want customers to re-authorise each payment manually.

**Template Style:** Choose between **Bootstrap 5** and **UIkit** to match your site's front-end template framework.

### Order Statuses

These settings control which order status J2Commerce applies at each stage of the payment lifecycle. GoCardless payments move through several stages because bank debit is asynchronous.

| Setting | What it controls | Default |
|---------|-----------------|---------|
| **Payment Confirmed Status** | Status set when GoCardless confirms the payment via webhook (`payments.confirmed` event). This is when the order is considered paid. | Confirmed (1) |
| **Payment Pending Status** | Status set immediately after the customer authorises the mandate and is returned to your store — before the payment clears. | Pending (7) |
| **Payment Settled Status** | Optional status to set when the payment is paid out to you (`payments.paid_out`). Leave empty for no change. | _(none)_ |
| **Payment Failed Status** | Status set when a payment fails, is cancelled, or is charged back. | Failed (6) |
| **Cancel Order Status** | Status set when you cancel a payment from the order admin screen. | Failed (6) |
| **Refund Order Status** | Status set when you issue a refund from the order admin screen. | _(none)_ |

### Surcharge

Add an optional handling fee when customers choose GoCardless.

**Surcharge Name:** Label shown to the customer for the fee (e.g., "Bank transfer fee")

**Surcharge Percent:** Percentage of the order total to add as a fee (e.g., `1.5` for 1.5%)

**Surcharge Fixed:** Fixed amount to add as a fee regardless of order size

**Surcharge Tax Class:** Tax profile to apply to the surcharge amount

Leave both percent and fixed fields empty to charge no surcharge.

### GeoZone Availability Restrictions

**GeoZone Restriction:** Limit this payment method to customers in a specific geozone — leave blank to allow all locations.

**Minimum Order Subtotal:** Hide GoCardless as a payment option when the cart subtotal is below this amount.

**Maximum Order Subtotal:** Hide GoCardless when the cart subtotal is above this amount.

### Custom HTML Snippets

**Thank-You Article:** Optional Joomla article to display on the order confirmation page after payment is authorised.

**On Selection Text:** Displayed when the customer selects GoCardless as their payment method at checkout.

**Before Payment Text:** Displayed just above the authorise button, before the customer is redirected to GoCardless.

**After Payment Text:** Displayed on the confirmation page after the customer returns from GoCardless.

**On Error Text:** Displayed when an error occurs during the payment flow.

**Order Cancelled Message:** Displayed when the customer cancels or navigates away.

### Dashboard Icon

**Show Dashboard Icon:** Add a quick-access icon for this plugin to the J2Commerce dashboard.

**Dashboard Icon Label:** Label for the icon. Defaults to "GoCardless Direct Debit" if left blank.

### Debug Mode

**Debug Mode:** Record detailed API request and response data to the Joomla log file at `administrator/logs/payment_gocardless.php`. Default is **No**.

Only enable this when diagnosing a specific problem. Disable it on live sites — debug logs can contain sensitive payment data.

## Setting Up Webhooks

:::warning

Configuring the webhook is essential. GoCardless payments are confirmed **asynchronously** — the customer's bank may take 2–5 business days to clear the payment. The webhook is how GoCardless tells your store that the payment has been confirmed (or failed). Without it, orders will stay in a pending status indefinitely.

:::

### Step 1: Copy your webhook URL

The webhook URL for your store follows this pattern:

```
https://your-site.com/index.php?option=com_ajax&format=raw&plugin=payment_gocardless&group=j2commerce&task=webhook
```

Replace `your-site.com` with your actual domain.

### Step 2: Register the endpoint in GoCardless

1. Log in to your GoCardless dashboard (sandbox or live, depending on which environment you are setting up).
2. Go to **Developers** -> **Webhooks**.
3. Click **Create endpoint**.
4. Paste your webhook URL into the **Endpoint URL** field.
5. GoCardless will display a **Signing secret** after saving. Copy it immediately — it is only shown once.

<!-- SCREENSHOT: GoCardless developer dashboard showing webhook endpoint creation form -->

### Step 3: Paste the signing secret into the plugin

1. In the Joomla Administrator, go to **J2Commerce** -> **Setup** -> **Payment Methods** -> **GoCardless Direct Debit**.
2. Paste the signing secret into the **Live Webhook Endpoint Secret** field (or **Sandbox Webhook Endpoint Secret** if configuring the sandbox).
3. Click **Save**.

The plugin uses this secret to verify every incoming webhook with an HMAC-SHA256 signature check. Requests that do not pass the check are silently ignored.

### Webhook events handled

| Event | What happens in J2Commerce |
|-------|---------------------------|
| `payments.confirmed` | Order status updated to **Payment Confirmed Status** — the order is paid |
| `payments.paid_out` | Order status updated to **Payment Settled Status** (if configured) |
| `payments.failed` | Order status updated to **Payment Failed Status** |
| `payments.cancelled` | Order status updated to **Payment Failed Status** |
| `payments.charged_back` | Order status updated to **Payment Failed Status** |
| `mandates.active` | Mandate marked as available for subscription renewals |
| `mandates.cancelled` / `mandates.failed` / `mandates.expired` | Mandate marked inactive; associated subscriptions cancelled |
| `refunds.created` / `refunds.paid` | Informational log entry (refund was already initiated by admin) |

## How Checkout Works for Customers

1. The customer adds items to the cart and proceeds to checkout.
2. On the payment step, **GoCardless Direct Debit** appears in the list of payment options. A notice explains that they will be redirected to GoCardless to authorise a Direct Debit and that payment takes 2–5 business days.
3. The customer clicks **Authorise Direct Debit** and is redirected to a GoCardless-hosted page.
4. On the GoCardless page, the customer enters their bank account details and approves the Direct Debit mandate.
5. GoCardless redirects the customer back to your store. They see a confirmation page showing their order reference and a message that the payment is being processed.
6. The order is placed with a **pending** status. The customer receives a notification once the payment clears (2–5 business days later), triggered by the GoCardless webhook.

<!-- SCREENSHOT: Checkout page showing the GoCardless payment option with the redirect notice -->

<!-- SCREENSHOT: Post-payment pending page showing order reference and payment status -->

## Understanding the Order Lifecycle

Because GoCardless bank payments are not instant, the order goes through these stages:

| Stage | Order status | What triggered it |
|-------|-------------|------------------|
| Customer authorises the mandate and returns to your store | **Pending** (or your configured pending status) | Customer redirect return |
| GoCardless confirms the payment has cleared | **Confirmed** (or your configured confirmed status) | `payments.confirmed` webhook — arrives 2–5 business days later |
| GoCardless pays the funds out to your bank account | No change (unless you configured a settled status) | `payments.paid_out` webhook |
| Payment fails or is charged back | **Failed** (or your configured failed status) | `payments.failed` / `payments.charged_back` webhook |

This is normal for Direct Debit and not a sign that anything went wrong. Store owners should expect a gap of 1–5 business days between the order being placed and it moving to a confirmed/paid status.

## Refunds

### Checking whether refunds are available

GoCardless creditor accounts do not have refunds enabled by default. Before you can issue a refund from J2Commerce, your GoCardless account must have the refund capability activated.

To check or request this:

1. Log in to your GoCardless dashboard.
2. Go to **Settings** -> **Creditor details**.
3. If refunds are not shown as enabled, contact GoCardless support to have them activated for your account.

### Issuing a refund from the order screen

Once your creditor account has refunds enabled:

1. Go to **J2Commerce** -> **Sales** -> **Orders** and open the relevant order.
2. In the payment section, click **Check Refund Availability** to verify that your creditor account can currently process refunds. The refund UI will appear if the check passes.

<!-- SCREENSHOT: J2Commerce order screen showing the Check Refund Availability button and refund form -->

3. Enter an amount in the **Refund amount** field for a partial refund, or leave it blank to refund the full payment amount.
4. Click **Issue Refund** and confirm the prompt.

The refund is sent to GoCardless and credited back to the customer's bank account. GoCardless typically takes 3–5 business days to process a refund.

:::info

You can only refund a payment that has been confirmed. If the payment is still in `pending_submission` or `submitted` status, use **Cancel Payment** instead (see below).

:::

### Cancelling a payment

A cancellation stops a payment before it has been submitted to the banking network.

1. Open the order in **J2Commerce** -> **Sales** -> **Orders**.
2. Click **Cancel Payment** and confirm the prompt.

Once a payment has been submitted to the bank, cancellation is no longer possible — use **Issue Refund** instead.

You can also click **Sync Payment Status** to pull the current status directly from GoCardless and update the order accordingly.

## Subscriptions and Recurring Payments

When **Allow Mandate Renewals** is enabled and you have the J2Commerce Subscriptions add-on installed, GoCardless can process renewal payments automatically using the stored mandate from the customer's initial order.

- The customer authorises a Direct Debit mandate once at their first checkout.
- On each renewal date, J2Commerce instructs GoCardless to collect the renewal amount against the stored mandate.
- No further action is needed from the customer.
- If the mandate is cancelled or expires (GoCardless sends a `mandates.cancelled` webhook), the subscription is cancelled automatically.

If you do not use subscriptions, the **Allow Mandate Renewals** setting has no effect on regular one-time orders.

## Going Live Checklist

Before accepting real payments, work through this checklist.

1. Create a live GoCardless account at [gocardless.com](https://gocardless.com) and complete identity verification.
2. Generate a live access token from **Developers** -> **Create access token**.
3. Note your live Creditor ID from **Settings** -> **Creditor details**.
4. Register a live webhook endpoint in the GoCardless dashboard and copy the signing secret.
5. In the plugin settings, set **Sandbox / Test Mode** to **No**.
6. Fill in **Live Access Token**, **Live Webhook Endpoint Secret**, and **Live Creditor ID**.
7. Click **Save**.
8. Place a small live test order using a real bank account to confirm the full flow works end-to-end.
9. Verify the order moves to your **Payment Confirmed Status** within 1–2 business days.
10. If you intend to issue refunds, confirm with GoCardless support that `can_create_refunds` is enabled on your creditor account.

:::warning

Remember to turn off **Sandbox / Test Mode** before accepting real orders. A warning banner on the J2Commerce dashboard will remind you if sandbox mode is still active.

:::

## Troubleshooting

### Orders are stuck in Pending — they never move to Confirmed

**Cause:** The webhook is not reaching your store, the endpoint secret is wrong, or the webhook was not registered in GoCardless.

**Solution:**

1. Confirm the webhook endpoint is registered in the GoCardless dashboard under **Developers** -> **Webhooks**. Check that the URL matches your store exactly, including `https://`.
2. Open the plugin settings and verify the **Webhook Endpoint Secret** (live or sandbox) exactly matches the signing secret shown in GoCardless — it is case-sensitive.
3. Make sure your store is publicly accessible over HTTPS. GoCardless cannot deliver webhooks to a site on localhost or behind a VPN.
4. Enable **Debug Mode** in the plugin settings, then check **System** -> **Logs** in the Joomla Administrator for any webhook-related errors.

### The Refund button does not appear, or "Check Refund Availability" shows refunds are not enabled

**Cause:** The GoCardless creditor account does not have the refund capability enabled. New accounts have this disabled by default.

**Solution:**

Contact GoCardless support and ask them to enable refunds (`can_create_refunds`) for your creditor account. Once enabled, click **Check Refund Availability** on the order again and the refund UI will appear.

### The customer is redirected to GoCardless but returns with no order created

**Cause:** The session expired during the GoCardless redirect, or the customer navigated away without completing the authorisation.

**Solution:**

Ask the customer to return to the cart and restart checkout. The GoCardless authorisation flow must be completed in a single session. If the problem is intermittent, check that your server session timeout is not set shorter than the time a typical customer takes to complete the bank authorisation page.

### Mandate failed or was cancelled unexpectedly

**Cause:** The customer's bank rejected the Direct Debit setup, or the customer cancelled the mandate through their online banking.

**Solution:**

1. The order will have moved to the **Payment Failed Status** automatically (via the `mandates.failed` or `mandates.cancelled` webhook).
2. Contact the customer and ask them to place a new order. They will need to re-authorise a new mandate.
3. In the UK, customers have the right to cancel a Direct Debit at any time through their bank. This is expected behaviour.

### Wrong currency or payment scheme error

**Cause:** GoCardless automatically selects the Direct Debit scheme based on the currency and country. If the store currency is not supported by any scheme on your creditor account, the payment cannot be collected.

**Solution:**

Check that your store currency matches a scheme your GoCardless creditor account supports (GBP → Bacs, EUR → SEPA, USD → ACH, AUD → BECS, CAD → PAD). If you trade in multiple currencies, verify that all relevant schemes are active on your GoCardless account.

### Missing credentials warning on the J2Commerce dashboard

**Cause:** The Access Token field is empty for the currently active environment (live or sandbox).

**Solution:**

1. Go to **J2Commerce** -> **Setup** -> **Payment Methods** -> **GoCardless Direct Debit**.
2. Fill in the **Live Access Token** (or **Sandbox Access Token** if sandbox mode is on).
3. Click **Save**.
