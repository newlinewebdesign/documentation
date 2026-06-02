---
title: "Restrict Payment Methods"
sidebar_label: "Restrict Payment Methods"
sidebar_position: 50
description: "Control which payment methods appear at checkout based on per-product rules and global keep or remove lists."
---

# Restrict Payment Methods

The **Restrict Payment Methods** app gives you precise control over which payment options your customers see at checkout. You can limit payment methods per product, whitelist a set of global methods, or permanently hide specific gateways — all without touching your payment plugin settings.

A common use case: you stock a mix of retail and wholesale products. You want PayPal for retail customers but Bank Transfer only for wholesale buyers. By tagging each product with its allowed methods, J2Commerce shows only the appropriate options the moment a buyer's cart contains a restricted product.

## Purchase and Download

The **Restrict Payment Methods** app is a separate add-on available from the [J2Commerce Extensions Store](https://www.j2commerce.com). It is not included with the core J2Commerce 6 component.

1. Go to the [J2Commerce website](https://www.j2commerce.com) and browse to **Extensions** -> **Apps**.
2. Locate **Restrict Payment Methods**, click **View Details**, add it to your cart, and complete checkout.
3. Go to your profile -> **My Downloads**, find the app, click **Available Versions** -> **View Files** -> **Download**.

## Install the App

1. In the Joomla admin, go to **System** -> **Install** -> **Extensions**.
2. Upload the `app_restrictpayment.zip` package file.
3. The plugin installs and enables automatically.

<!-- SCREENSHOT: System > Install > Extensions upload screen with app_restrictpayment.zip selected -->

## Enable the App

After installation, verify the app is enabled before configuring it.

**Option A:** Go to the **J2Commerce** icon in the top toolbar -> **Apps**. Search for **Restrict Payment Methods**, click the toggle to turn it green.

**Option B:** Go to **Components** -> **J2Commerce** -> **Apps**, find the app, and enable it.

<!-- SCREENSHOT: J2Commerce > Apps list with Restrict Payment Methods toggle highlighted -->

## How the Rules Work Together

This app applies three layers of filtering in a specific order at checkout. Understanding the order prevents unexpected results.

1. **Per-product whitelist** — Each product can list which payment methods are allowed when it is in the cart. When the cart contains multiple restricted products, the app builds the **union** of all allowed methods. A product set to **All payment methods** does not restrict the list.

2. **Global keep list (whitelist)** — If you configure **Keep only these payment methods** in the plugin settings, only methods in that list can survive to checkout — regardless of per-product settings. Think of this as a store-wide ceiling.

3. **Global remove list (blacklist)** — Any method listed in **Always remove these payment methods** is stripped last, unconditionally. This is ideal for hiding a gateway you no longer support without disabling it entirely.

If none of the three layers contain any rules, the app does nothing and all enabled payment methods show normally.

:::tip Rule interaction summary

Per-product whitelist → then global keep list → then global remove list. A method must pass all three steps to appear.

:::

## Configure Global Settings

Go to **J2Commerce** -> **Apps** and click the **Restrict Payment Methods** title to open the plugin settings.

<!-- SCREENSHOT: Plugin Manager edit screen for J2Commerce - Restrict Payment Methods, showing Payment Restrictions fieldset -->

### Payment Restrictions fieldset

| Field | What it does | Default |
|-------|--------------|---------|
| **Keep only these payment methods** | If any methods are selected, only those methods can appear at checkout. Leave empty to allow all (subject to other rules). | Empty (no restriction) |
| **Always remove these payment methods** | Selected methods are hidden at checkout unconditionally — regardless of product or keep rules. | Empty (nothing removed) |
| **Debug logging** | Writes filtering decisions to `administrator/logs/plg_j2commerce_app_restrictpayment.log.php`. Disable on live stores. | No |

:::tip Toggle Inline Help

Click the **Toggle Inline Help** button at the top right of the plugin edit screen to show a description beneath each field.

:::

After changing settings, click **Save & Close**.

## Set Per-Product Payment Restrictions

Per-product rules are configured directly on each product's article, in the J2Commerce Apps tab.

1. Go to **Content** -> **Articles** and open the product article you want to restrict.
2. Click the **J2Commerce** tab, then the **App** sub-tab.
3. Find the **Payment Restrictions** fieldset.
4. In the **Allowed payment methods** field, select one or more payment methods. To remove any restriction for this product, select **All payment methods**.
5. Click **Save**.

<!-- SCREENSHOT: Product article edit screen, J2Commerce tab > App sub-tab, showing the Allowed payment methods multi-select field -->

The selector lists every **enabled** payment plugin on your site. If a payment method you expect is missing, check that it is published in **J2Commerce** -> **Payments** -> **Payment Methods**.

:::info Multiple products in cart

When the cart contains more than one restricted product, the app uses the **union** of their allowed methods. For example: Product A allows `payment_banktransfer` and Product B allows `payment_paypal` — customers with both in their cart see both Bank Transfer and PayPal.

A product with **All payment methods** selected contributes no restriction. Only products with specific methods selected reduce the available list.

:::

## Worked Examples

### Example 1: Bank Transfer only for wholesale products

**Goal:** Wholesale products must only be purchasable via Bank Transfer. Regular products should show all methods.

**Setup:**

1. Open each wholesale product article, go to **J2Commerce** -> **App** -> **Payment Restrictions**.
2. Set **Allowed payment methods** to **Bank Transfer** only.
3. Leave all retail products set to **All payment methods**.

**Result:** A cart containing only wholesale products shows Bank Transfer. A cart containing only retail products shows all methods. A mixed cart shows Bank Transfer (the only method in the intersection of the wholesale product's allowed set and the rest of the cart).

### Example 2: Hide a legacy payment gateway globally

**Goal:** You stopped using a gateway but cannot disable it yet because of pending orders. You want to hide it from new customers.

**Setup:**

1. Open the **Restrict Payment Methods** plugin settings.
2. In **Always remove these payment methods**, select the gateway.
3. Save.

**Result:** The gateway no longer appears at checkout for any order, regardless of product settings.

### Example 3: Restrict the entire store to two payment methods

**Goal:** You only accept PayPal and Bank Transfer, and want to ensure no other method ever appears even if a new payment plugin is accidentally enabled.

**Setup:**

1. Open plugin settings.
2. In **Keep only these payment methods**, select **PayPal** and **Bank Transfer**.
3. Save.

**Result:** Only those two methods can appear at checkout. Any other enabled payment plugin is hidden automatically.

## Migrating from J2Store 4

If you are upgrading from J2Store 4, note the following changes.

### Per-product and global rules

The core restriction behaviour — per-product whitelists, global keep list, and global remove list — works the same way as in J2Store 4. No manual migration of product data is needed. The existing `payment_restrictions` values stored in product params carry forward automatically.

### Price-band filtering is now a core feature

In J2Store 4, this plugin included a **price band** feature: you could set a minimum and maximum order total per payment method to control when it appeared. This feature is **not** part of the J2Commerce version of the plugin because J2Commerce 6 includes equivalent functionality built into the core.

To set order total limits on a payment method in J2Commerce 6:

1. Go to **J2Commerce** -> **Payments** -> **Payment Methods**.
2. Click the payment method you want to configure.
3. Set the **Minimum order subtotal** and **Maximum order subtotal** fields.
4. Optionally, assign a **Geo Zone** to limit the method by customer location.
5. Save.

J2Commerce applies these limits automatically at checkout without needing any extra plugin.

## Troubleshooting

### No payment methods appear at checkout

**Cause:** The active restrictions have eliminated every available method. This usually means the per-product whitelist and the global keep list do not overlap, leaving an empty set.

**Solution:**

1. Enable **Debug logging** in the plugin settings and reproduce the problem. Open `administrator/logs/plg_j2commerce_app_restrictpayment.log.php` and look for the filtering summary line (for example, `onGetPaymentPlugins: 3 → 0`).
2. Check which products in the cart have a restricted **Allowed payment methods** setting. The union of those must contain at least one method.
3. Check the global **Keep only these payment methods** list. If it is set, the methods allowed by per-product rules must also appear in this list.
4. Make sure at least one method survives the **Always remove** list.
5. Confirm that the surviving method is published in **J2Commerce** -> **Payments** -> **Payment Methods**.

### A payment method I restricted still appears

**Cause:** The product's per-product restriction is not saved, or the app is not enabled.

**Solution:**

1. Confirm the app is enabled in **J2Commerce** -> **Apps** (green toggle).
2. Open the product article, navigate to **J2Commerce** -> **App** -> **Payment Restrictions**, and verify the **Allowed payment methods** field shows the expected selection — not **All payment methods**.
3. Save the product and test again.

### The payment method selector on the product is empty

**Cause:** No payment plugins are currently enabled on the site.

**Solution:** Go to **J2Commerce** -> **Payments** -> **Payment Methods** and confirm at least one payment method is published. The selector only lists enabled payment plugins.

### Debug logging

Enable **Debug logging** in the plugin settings to write detailed filtering decisions to the Joomla log. The log file is located at:

```
administrator/logs/plg_j2commerce_app_restrictpayment.log.php
```

Each checkout that triggers the filter writes a line showing how many payment methods were considered and how many remained after filtering. Disable debug logging on production sites once you have resolved your issue.

## Related Topics

- [Payment Methods](../../payment-methods/index.md)
- [Product Shipping Restrictions](./app-restrictbyshipping.md)
