---
title: "Incremental Pricing"
sidebar_label: "Incremental Pricing"
sidebar_position: 5
description: "Set quantity-based per-unit price tiers on individual products — and on individual variants — so the graduated saving applies automatically as a labelled discount line in the cart."
---

# Incremental Pricing

The Incremental Pricing app lets you reward customers who order more of the same product by automatically reducing the per-unit price at defined quantity thresholds. You set the tiers once on each product (or on individual variants), and J2Commerce takes care of calculating the saving and showing it as a clean discount line in the cart — no coupons or manual adjustments required.

This is ideal for wholesale-style pricing, volume incentives, or any scenario where the effective unit cost should drop as order size grows.

## How It Works

You define a set of quantity tiers on a product. Each tier says: "once the ordered quantity reaches this number, each unit in the order costs this amount." When a shopper adds enough items to trigger a tier, the plugin:

1. Calculates the graduated cost of the entire order line across all applicable tiers.
2. Subtracts that graduated cost from the regular line total.
3. Applies the difference as an automatic discount to the order, labelled with your configured display text.

### Worked Example

A product has a regular price of **$10.00**. You configure two tiers:

| Quantity | Per-Unit Price |
|----------|----------------|
| 5 and above | $8.00 |
| 10 and above | $6.00 |

- A shopper who orders **3 units** pays the regular price: 3 × $10.00 = **$30.00**. No discount applies because the quantity is below the first tier.
- A shopper who orders **7 units** pays: units 1–4 at $10.00 each, plus units 5–7 at $8.00 each. That is (4 × $10.00) + (3 × $8.00) = $40.00 + $24.00 = **$64.00** — a saving of $6.00 versus the full regular price of $70.00.
- A shopper who orders **12 units** pays: units 1–4 at $10.00, units 5–9 at $8.00, units 10–12 at $6.00. That is $40.00 + $40.00 + $18.00 = **$98.00** — a saving of $22.00 versus the regular price of $120.00.

The saving always appears as a separate discount line in the cart and order totals, making it visible and easy to understand.

## Installation

This plugin is a separate add-on available from the [J2Commerce Extensions Store](https://www.j2commerce.com). It is not included with the core J2Commerce 6 component.

1. Purchase and download the `app_incrementalprice.zip` package from the J2Commerce website.
2. Go to **System** -> **Install** -> **Extensions**.
3. Upload the `app_incrementalprice.zip` package file.
4. The plugin installs and enables automatically.

<!-- SCREENSHOT: Joomla Extensions installer showing the app_incrementalprice.zip upload confirmation screen -->

After installation, configure the plugin by going to **J2Commerce** -> **Apps** and clicking **Incremental Pricing**.

## Global Plugin Settings

These settings apply across your entire store. Go to **J2Commerce** -> **Apps**, then click **Incremental Pricing** to open the plugin settings.

<!-- SCREENSHOT: The Incremental Pricing plugin settings panel in the J2Commerce Apps screen, showing the Basic Settings and Product Page Display fieldsets -->

### Basic Settings

| Setting | Description | Default |
|---------|-------------|---------|
| **Discount Display Text** | The label shown for the incremental pricing saving in the order totals — for example "Volume Discount" or "Bulk Saving". Leave blank to use the default label "Incremental Pricing". | _(empty — uses default)_ |
| **Debug Mode** | Set to **Yes** to write detailed discount calculation logs to the Joomla log directory. The log file is named `app_incrementalprice.php`. Disable this in a live store. | No |

### Product Page Display

These settings control the optional storefront message that nudges shoppers toward the next discount tier. See [Front-End Tier Nudge Messages](#front-end-tier-nudge-messages) for a full explanation of what the shopper sees.

| Setting | Description | Default |
|---------|-------------|---------|
| **Show Message on Product Page** | Set to **Yes** to display a "Buy More Save More" incentive message on the product detail page. | No |
| **Message Position** | Choose whether the message appears **Before the Add to Cart button** or **After the Add to Cart button**. | After the Add to Cart button |
| **Frontend Subtemplate** | Choose the framework subtemplate used to render the product-page message. Select the one that matches your storefront theme: **bootstrap5** or **uikit**. | bootstrap5 |

Click **Save** after making any changes.

## Setting Up Incremental Pricing on a Product

You configure quantity tiers on each product individually. This means you can have incremental pricing on some products while others use regular pricing.

### Step 1: Open the Product Editor

1. Go to **J2Commerce** -> **Catalog** -> **Products**.
2. Click the name of the product you want to edit.

<!-- SCREENSHOT: The J2Commerce Products list view with a product row highlighted -->

### Step 2: Find the Incremental Pricing Tab

In the product editor, scroll down or look for the **Incremental Pricing** section among the product app tabs.

<!-- SCREENSHOT: The product editor showing the Incremental Pricing tab or section panel -->

### Step 3: Enable Incremental Pricing

Set **Enable Incremental Pricing** to **Yes**.

<!-- SCREENSHOT: The Enable Incremental Pricing toggle set to Yes -->

### Step 4: Add Quantity Tiers

The **Quantity Tiers** table appears once you enable incremental pricing. Each row in the table defines a threshold and a per-unit price for that threshold.

<!-- SCREENSHOT: The Quantity Tiers table with two example rows filled in, showing the Qty column and the Price column -->

To add a tier:

1. Click **Add Incremental Price** to insert a new row.
2. In the **Qty +** column, enter the minimum quantity at which this per-unit price applies. For example, enter `5` to mean "5 and above".
3. In the **Price** column, enter the per-unit price charged once the ordered quantity reaches that threshold. For example, enter `8.00`.
4. Repeat for each additional tier you need.

To remove a tier, click the **Delete** button (trash icon) on that row.

### Tier Pricing Rules

- **Tiers must be lower than the regular product price.** If any tier price exceeds the regular product price, J2Commerce blocks the save and shows an error. A tier price higher than the regular price would increase cost rather than reduce it, so the plugin prevents this.
- **Units below the lowest tier are charged the regular product price.** For example, if your lowest tier starts at quantity 5, then orders of 1–4 units use the product's regular price.
- **Tiers are graduated, not stepped.** The price change applies unit-by-unit as the quantity crosses each threshold, not retroactively to the entire order.

### Step 5: Save the Product

Click **Save** or **Save & Close** to apply your tier configuration.

<!-- SCREENSHOT: The product editor Save button in the toolbar, with the Incremental Pricing section visible below -->

## Per-Variant Incremental Pricing

For variable products (products with options such as size or color), you can set independent incremental pricing tiers on individual variants. This is useful when variant prices differ significantly and you want the discount thresholds or per-unit prices to reflect each variant's own price point.

### How Variant Tiers Override Product Tiers

When a shopper adds a variable product to their cart:

1. J2Commerce checks whether the selected variant has its own incremental pricing enabled.
2. If **yes**, the variant's own tiers are used for the discount calculation. The product-level tiers are ignored for that variant.
3. If **no**, the product-level tiers apply as normal.

This means you can configure a base set of tiers on the product that applies to most variants, and then override specific variants (such as a premium size or a higher-cost option) with their own separate tier structure.

### Setting Up Tiers on a Variant

1. Go to **J2Commerce** -> **Catalog** -> **Products** and open a variable product.
2. Click the **Variants** tab to see the list of variants.
3. Click the variant you want to configure, or click **Edit** to open the variant editor.
4. In the variant editor, scroll to the **Incremental Pricing** fieldset.

<!-- SCREENSHOT: Variant editor screen with the Incremental Pricing fieldset visible, showing the enable toggle and the Quantity Tiers table -->

5. Set **Enable Incremental Pricing** to **Yes**.
6. Add quantity tiers the same way you would on the product: click **Add Incremental Price**, then enter a **Qty +** threshold and a **Price** for each tier.
7. Click **Save** to save the variant.

The same pricing rules apply at the variant level: every tier price must be lower than that variant's own regular price.

## Front-End Tier Nudge Messages

When **Show Message on Product Page** is enabled in the plugin settings, shoppers on the product detail page see an information notice that tells them how much they can save by adding more items. This nudge is designed to encourage larger orders.

### What the Shopper Sees

The message reads:

> **Buy More Save More: Save [amount] for every item over [quantity]!**

For example, if a product has tiers set at 5 units ($8.00 each) and 10 units ($6.00 each), and the regular price is $10.00, the message initially shows the lowest tier that the shopper has not yet reached:

> Buy More Save More: Save $2.00 for every item over 5!

### How the Message Updates

The message is live — it reacts to the shopper's actions on the page without requiring a page reload:

- **As the shopper changes the quantity:** The message always reflects the next tier above the quantity currently entered. When the shopper reaches a tier, the message advances to show the next one. Once the shopper's quantity meets or exceeds the highest tier, the message hides automatically because no further saving is available.
- **As the shopper changes a variant option:** When the shopper selects a different size, color, or other variant, the message updates immediately to reflect that variant's effective tiers. If the new variant has its own tiers enabled, those are shown. If not, the product-level tiers apply. If neither applies, the message hides.

### Message Position

You control where the message appears on the product page using the **Message Position** setting in the plugin's **Product Page Display** fieldset:

- **Before the Add to Cart button** — the message appears directly above the button.
- **After the Add to Cart button** — the message appears directly below the button.

### Enabling the Message

The message feature is off by default. To turn it on:

1. Go to **J2Commerce** -> **Apps** and click **Incremental Pricing**.
2. In the **Product Page Display** fieldset, set **Show Message on Product Page** to **Yes**.
3. Choose a **Message Position** and a **Frontend Subtemplate** that matches your theme.
4. Click **Save**.

<!-- SCREENSHOT: Plugin settings screen showing the Product Page Display fieldset with Show Message on Product Page set to Yes -->

The message only appears on the product detail page (not on category listing pages) and only for products that have incremental pricing tiers configured.

## How the Saving Appears in the Cart and Order

When a shopper adds enough of the product to trigger a tier, they will see:

- The product line item showing the standard per-unit price and quantity.
- A separate discount line below the subtotal, labelled with your configured **Discount Display Text** (default: "Incremental Pricing"), showing the total saving as a negative amount.
- The order total reflecting the reduced cost.

<!-- SCREENSHOT: A cart totals section showing the product line, the "Incremental Pricing" discount line with a negative value, and the final order total -->

The discount also appears on the order confirmation page and in the admin order detail view, making it clear to both the customer and your store staff why the total was reduced.

## Interaction with Coupons and Vouchers

Incremental pricing and coupons (or vouchers) do not combine. If a shopper applies a coupon or voucher code to their cart, the incremental pricing discount is automatically suppressed for that order. This prevents customers from stacking a volume discount on top of a coupon discount.

If a customer reports that they are not receiving an incremental pricing discount, check whether they have an active coupon or voucher applied to their cart — this is the most common cause.

## What Changed from the J2Store Version

If you used the Incremental Pricing app with J2Store, the core pricing logic is the same. The main changes are:

- **Saving always appears as a labelled discount line.** The previous J2Store version included an "apply as product price" mode that folded the graduated cost directly into the displayed product price instead of showing a separate discount. This mode has been retired in J2Commerce 6 because J2Commerce computes line subtotals natively from the unit price and quantity. Showing the saving as an explicit discount line is clearer for customers and compatible with J2Commerce's order total pipeline.
- **Per-variant tiers are now supported.** Individual variants of a variable product can have their own independent tier structure, overriding the product-level tiers when a matching variant is added to the cart.
- **Live product-page nudge messages.** The optional storefront message that shows shoppers the next available saving tier was not available in the J2Store version. It is new in J2Commerce 6.
- **Tax handling is automatic.** If the product has a tax profile assigned, J2Commerce calculates the tax portion of the discount correctly and adjusts the discount amount so the tax is handled consistently with the rest of your store's configuration.

## Troubleshooting

### The discount is not showing in the cart

**Cause:** The most common reason is an active coupon or voucher. Incremental pricing is suppressed whenever a coupon or voucher is applied to the cart.

**Solution:**
1. Ask the customer to remove any coupon or voucher from the cart.
2. Confirm that **Enable Incremental Pricing** is set to **Yes** on the product (or on the relevant variant) in the product editor.
3. Confirm that the ordered quantity meets or exceeds the lowest tier threshold.

### The product save fails with a price error

**Cause:** One or more of the tier prices is set higher than the product's regular price (or higher than the variant's own price for variant-level tiers).

**Solution:**
1. Open the product editor and check the **Price** field.
2. In the **Quantity Tiers** table, ensure every tier price is lower than the regular price.
3. If the product price has not yet been set (or is zero), enter a regular price before saving tiers.
4. If the error mentions a variant, open that variant's editor and check its tier prices against its own price.

### The product-page nudge message is not showing

**Cause:** The **Show Message on Product Page** setting may be off, or the product may not have any tiers configured, or the wrong **Frontend Subtemplate** is selected.

**Solution:**
1. Go to **J2Commerce** -> **Apps** -> **Incremental Pricing** and confirm **Show Message on Product Page** is set to **Yes**.
2. Confirm the **Frontend Subtemplate** matches your storefront theme (bootstrap5 or uikit).
3. Open the product in the product editor and verify that **Enable Incremental Pricing** is **Yes** and at least one tier is saved.
4. The message only appears on the product detail page (the single-product view), not on category listing pages.

### The nudge message does not update when a variant is selected

**Cause:** The variant may not have its own tiers, or the product-level tiers may not apply to that variant's price point.

**Solution:**
1. If a specific variant should show different tiers, open that variant in the variant editor and enable incremental pricing with its own tiers.
2. If the product-level tiers should apply, make sure the variant does not have **Enable Incremental Pricing** set to **Yes** with an empty tiers table — an enabled-but-empty variant configuration is treated as "no tiers" for that variant.

### Discount calculations appear incorrect

**Cause:** Tiers may not be configured as expected, or debug mode may help reveal the calculation steps.

**Solution:**
1. Go to **J2Commerce** -> **Apps** -> **Incremental Pricing** and set **Debug Mode** to **Yes**.
2. Reproduce the cart scenario.
3. Check the Joomla log file at `[joomla-root]/logs/app_incrementalprice.php` for a detailed breakdown showing the saving, tax amount, quantity, regular cost, and graduated cost.
4. Turn **Debug Mode** off again once you have finished investigating.

## Related Topics

- [Bulk Discounts](./app-bulkdiscount.md)
- [Quantity Discounts](./qtydiscount.md)
- [Products](../../catalog/index.md)
