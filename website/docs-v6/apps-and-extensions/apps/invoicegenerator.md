---
title: "Invoice Number Generator"
sidebar_label: "Invoice Number Generator"
sidebar_position: 50
description: "Automatically generate sequential, prefix-based invoice numbers when orders reach selected statuses, and manually edit or regenerate invoice details from the admin order view."
---

# Invoice Number Generator

The Invoice Number Generator app assigns a unique, sequential invoice number to each order the moment it reaches a status you choose — such as Confirmed or Completed. Invoice numbers are formatted with a configurable prefix, optional zero-padding, and an invoice creation date, all without you lifting a finger. Admins can also open any order and manually override the prefix, number, or date, or trigger a fresh number on demand.

This plugin is a separate add-on available from the [J2Commerce Extensions Store](https://www.j2commerce.com). It is not included with the core J2Commerce component.

---

## Installation

1. Purchase and download the `plg_j2commerce_app_invoicegenerator.zip` package from the J2Commerce website.
2. Go to **System** -> **Install** -> **Extensions** in the Joomla administrator.
3. Upload the zip file using the **Upload Package File** tab.
4. The plugin installs automatically. You will see a confirmation message.
5. Go to **System** -> **Manage** -> **Plugins**.
6. Search for **Invoice Number Generator**.
7. Click the status icon to enable it (it should turn green).

<!-- SCREENSHOT: Plugin Manager search results showing the Invoice Number Generator plugin enabled -->

Once enabled, configure the plugin from **J2Commerce** -> **Apps** and click **Invoice Number Generator**.

<!-- SCREENSHOT: J2Commerce Apps screen with Invoice Number Generator tile highlighted -->

---

## How Auto-Generation Works

When an order's status changes, the plugin checks whether the new status matches any of the statuses you selected in **Trigger Status**. If it matches and the order does not already have an invoice number assigned under the same prefix, the plugin:

1. Queries the order database for the highest existing invoice number that uses the same prefix.
2. Adds 1 to produce the next sequential number.
3. Saves the invoice number, prefix, and invoice creation date to the order record.

The invoice date is derived from the order's own creation date, formatted with the **Date Format** setting. This means the invoice timestamp reflects when the order was originally placed, not when the status changed.

If the same prefix has never been used before, numbering starts at 1.

**One assignment per prefix.** If an order already has an invoice number under the configured prefix, the plugin skips it — no duplicate is created. If you change the prefix later and want to assign a new number under the new prefix, use the Regenerate button in the order.

---

## Settings Reference

Open the plugin from **J2Commerce** -> **Apps** -> **Invoice Number Generator** to reach the settings panel.

<!-- SCREENSHOT: Invoice Number Generator settings panel showing all fields in the Basic Settings tab -->

### Basic Settings

| Setting | Description | Default |
|---------|-------------|---------|
| **Show Invoice Edit Box** | Show the Invoice section in the admin order sidebar. Set to **No** to hide invoice fields from all orders. | Yes |
| **Allow Custom Prefix** | Allow admins to edit the invoice prefix directly in the order sidebar. | Yes |
| **Allow Custom Number** | Allow admins to type an arbitrary invoice number in the order sidebar. | Yes |
| **Allow Custom Date** | Allow admins to change the invoice date in the order sidebar using a date picker. | Yes |
| **Invoice Prefix** | The prefix prepended to every invoice number. For example, `INVG-` produces invoice numbers like `INVG-1`, `INVG-2`, and so on. Numbering is tracked separately per prefix, so changing this value starts a fresh sequence. | `INVG-` |
| **Number Padding Width** | Zero-pads the invoice number to this many digits. A value of `5` turns invoice number `42` into `00042`. Set to `0` to disable padding. | `0` |
| **Date Format** | PHP date format string used for the invoice creation date. Common formats: `Y-m-d` (2026-01-30), `d/m/Y` (30/01/2026), `F j, Y` (January 30, 2026). | `Y-m-d` |
| **Display Invoice For Order Status** | Which order statuses allow the invoice box to appear in the admin order view. Leave empty to show the box on all orders regardless of status. | _(empty — all statuses)_ |
| **Allow Regenerate** | Show a **Regenerate Invoice** button in the order sidebar. Clicking it overwrites the current invoice number and date with a freshly generated set. Disabled by default to prevent accidental overwrites. | No |

### Auto-Generation Section

| Setting | Description | Default |
|---------|-------------|---------|
| **Auto-Generate Invoices** | Master switch for automatic generation. When set to **No**, the plugin never assigns invoice numbers on status change — only manual assignment from the order sidebar is possible. | Yes |
| **Trigger Status** | One or more order statuses that trigger automatic invoice number assignment. When an order moves into any of these statuses, the plugin generates the number. Leave empty to trigger on every status change. | _(empty)_ |

### Advanced Tab

| Setting | Description | Default |
|---------|-------------|---------|
| **Debug** | Write generation and error events to `logs/app_invoicegenerator.php`. Useful for diagnosing why an invoice number was or was not assigned. Errors are always logged regardless of this setting. | No |

---

## Choosing Your Trigger Status

The **Trigger Status** setting controls which order status causes an invoice number to be assigned automatically. Most stores use a single status — typically the one that represents a confirmed, paid order.

Common choices:

- **Confirmed** — assign a number as soon as payment is acknowledged.
- **Completed** — assign a number only after the order is fully fulfilled.

You can select multiple statuses if your workflow requires it. The plugin skips assignment if the order already has a number under the current prefix, so selecting both Confirmed and Completed is safe — the number is assigned on whichever transition happens first.

---

## Manually Editing Invoice Details on an Order

When **Show Invoice Edit Box** is enabled, a card titled **Invoice** appears in the right sidebar of the admin order view for every order that matches the **Display Invoice For Order Status** selection.

<!-- SCREENSHOT: Admin order view showing the Invoice card in the right sidebar with prefix, number, date fields and Update Invoice button -->

The card shows fields according to what you have allowed:

- **Invoice Prefix** — a text field showing the current prefix. Visible when **Allow Custom Prefix** is **Yes**.
- **Invoice Number** — a text field showing the current number (zero-padded if padding is configured). Visible when **Allow Custom Number** is **Yes**.
- **Invoice Date** — a date picker showing the current invoice creation date. Visible when **Allow Custom Date** is **Yes**.

Click **Update Invoice** to save your changes. The page reloads to confirm the update was applied.

If **Allow Regenerate** is enabled, a second button, **Regenerate Invoice**, appears next to **Update Invoice**. Clicking it discards the existing invoice number and assigns a new sequential number using the current plugin prefix and today's date.

---

## Using the `[INVOICE_CREATED_DATE]` Tag in Emails

You can display the invoice creation date in any J2Commerce email template using the tag:

```
[INVOICE_CREATED_DATE]
```

The tag is replaced with the date stored on the order, formatted according to the **Date Format** setting. If no invoice date has been recorded on the order, the tag resolves to an empty string.

**Where to use it:** Go to **J2Commerce** -> **Configuration** -> **Emails** (or your email template manager) and add the tag anywhere in the message body — for example:

```
Invoice date: [INVOICE_CREATED_DATE]
```

This works in order confirmation emails, invoice emails, and any other template that runs through the J2Commerce email tag processor.

---

## Recommended Configuration for Most Stores

1. Set **Invoice Prefix** to your desired code — for example, your company abbreviation followed by the year: `ACME2026-`.
2. Set **Number Padding Width** to `5` so invoice numbers display as `ACME2026-00001` rather than `ACME2026-1`. This keeps lists sorted correctly and gives professional-looking numbers.
3. Set **Date Format** to the format your locale or accounting software expects.
4. Under **Trigger Status**, select only your "Confirmed" or equivalent paid status. This ensures an invoice number is assigned precisely once, at payment.
5. Leave **Allow Regenerate** set to **No** unless your workflow requires manual correction of invoice numbers.

---

## Troubleshooting

### Invoice number is not being assigned automatically

**Possible cause 1:** **Auto-Generate Invoices** is set to **No**.

**Solution:** Open the plugin settings and confirm **Auto-Generate Invoices** is set to **Yes**.

**Possible cause 2:** No statuses are selected in **Trigger Status**, but the field was previously saved with an empty value that the plugin treats as "never match."

**Solution:** Check the **Trigger Status** field. If it is empty, the plugin will trigger on any status change. If you believe a specific status should be triggering but is not, add it explicitly.

**Possible cause 3:** The order already has an invoice number assigned under the current prefix.

**Solution:** The plugin skips re-assignment intentionally. If you need a new number, use the **Regenerate Invoice** button in the order sidebar (requires **Allow Regenerate** set to **Yes**).

**Possible cause 4:** The plugin is not enabled.

**Solution:** Go to **System** -> **Manage** -> **Plugins**, search for **Invoice Number Generator**, and confirm the status is enabled (green).

---

### The Invoice card does not appear in the order sidebar

**Possible cause 1:** **Show Invoice Edit Box** is set to **No**.

**Solution:** Open the plugin settings and enable **Show Invoice Edit Box**.

**Possible cause 2:** The **Display Invoice For Order Status** field contains a list of statuses, and the current order is not in one of those statuses.

**Solution:** Either add the relevant status to the **Display Invoice For Order Status** list, or clear the field entirely so the box displays for all orders.

---

### The `[INVOICE_CREATED_DATE]` tag appears literally in the email

**Cause:** The order has not yet had an invoice date assigned — no invoice number has been generated for it.

**Solution:** Trigger invoice generation by moving the order to a status listed in **Trigger Status**, or manually save an invoice date using the order sidebar. Once the date is stored, the tag will resolve correctly on the next email send.

---

## What Is New vs the J2Store Version

If you used the Invoice Number Generator in J2Store 4, here is what has changed in J2Commerce 6:

| Area | J2Store 4 | J2Commerce 6 |
|------|-----------|--------------|
| **Framework** | FOF 2, non-namespaced PHP | Native Joomla 6 MVC, namespaced PHP 8.3 |
| **JavaScript** | jQuery-dependent | Vanilla ES6, no jQuery |
| **AJAX security** | Variable CSRF implementation | All AJAX calls require a valid Joomla CSRF token and admin authorisation check |
| **Sequential numbering** | Global sequential number | Per-prefix sequential numbering — changing the prefix starts a new independent sequence |
| **Order sidebar** | Rendered via legacy FOF templates | Injected via the `onJ2CommerceAfterAdminOrderSidebar` event into the Bootstrap 5 order view |
| **Logging** | Limited or none | Errors always logged; debug logging to `logs/app_invoicegenerator.php` via the Debug toggle |
| **Auto-generation trigger** | Status-based | Status-based, with IDOR protection to prevent cross-order manipulation |
