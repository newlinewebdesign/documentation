---
title: "Minimum Order Restriction"
sidebar_label: "Minimum Order"
sidebar_position: 40
description: "Block customers from completing checkout when the cart amount, quantity, or category value falls below your configured minimums."
---

# Minimum Order Restriction

The Minimum Order Restriction app lets you set rules that a shopper's cart must satisfy before they can proceed to checkout. You can require a minimum cart value, enforce minimum or maximum item quantities, and set category-specific spending thresholds — all without touching any code. If a shopper's cart falls short, they are returned to the cart page with a clear warning explaining what needs to change.

## Requirements

- PHP 8.3.0+
- Joomla! 6.x
- J2Commerce 6.x

## Purchase and Download

**Step 1:** Go to our [**J2Commerce** website](https://www.j2commerce.com/) **->** **Apps**

**Step 2:** Locate the **Minimum Order Restriction** App **->** click **View Details** **->** **Add to cart -> Checkout**.

**Step 3:** Go to your **My Downloads** under your profile button at the top right corner and search for the app. Click **Available Versions -> View Files -> Download Now**

## Install the App

This app is a separate add-on available from the [J2Commerce Extensions Store](https://www.j2commerce.com). It is not included with the core J2Commerce 6 component.

1. Purchase and download the `app_minimumorder.zip` package from the J2Commerce website.
2. In the Joomla admin, go to **System -> Install -> Extensions**.
3. Upload the plugin ZIP file or use the Install from URL option.

<!-- SCREENSHOT: System -> Install -> Extensions upload screen -->

## Enable the App

Once installed, you need to enable the app. There are **two** ways to access it.

**Option A:** Go to the **J2Commerce** icon at the top right corner **-> Apps**

**Option B:** Go to **Components** on the left sidebar **-> J2Commerce -> Apps**

<!-- SCREENSHOT: J2Commerce Apps list showing Minimum Order Restriction -->

Look for **Minimum Order Restriction**, click the **X**, and it will turn into a green checkmark. The app is now enabled and ready to configure.

## Configure the App

Click on the **Minimum Order Restriction** title next to the green checkmark to open the settings.

:::tip

**Tip:** Click the **Toggle Inline Help** button on any app to reveal a description below each field.

:::

The settings are split into four tabs: **Price-Based Settings**, **Quantity-Based Settings**, **Category-Based Settings**, and **Display**.

---

### Price-Based Settings tab

<!-- SCREENSHOT: Price-Based Settings fieldset in plugin configuration -->

This tab controls whether a minimum cart value is required before checkout.

| Field | Description | Default |
|-------|-------------|---------|
| **Enable Price-Based Restriction** | Turn the minimum order value check on or off. | Yes |
| **Calculation Type** | Which amount to compare against your minimum: **Subtotal** (products only, before shipping and tax) or **Total** (the full order amount). | Subtotal |
| **Include Discount in Order Total?** | Visible when **Calculation Type** is set to **Total**. Choose **Include Discount** to apply the check after any coupon is deducted, or **Exclude Discount** to check the total before discounts. | Include Discount |
| **Minimum Subtotal / Total** | The minimum amount the cart must reach. Shoppers below this value are returned to the cart with a warning. Set to `0` to disable the check without turning off the whole section. | 0 |
| **Debug Mode** | Write blocking decisions to the Joomla log directory. Disable in production. | No |

**Example:** Set **Calculation Type** to **Subtotal** and **Minimum Subtotal / Total** to `50` to require at least $50 in products before checkout can proceed.

---

### Quantity-Based Settings tab

<!-- SCREENSHOT: Quantity-Based Settings fieldset in plugin configuration -->

This tab controls how many items (total or per product line) must be in the cart.

| Field | Description | Default |
|-------|-------------|---------|
| **Enable Quantity-Based Restriction** | Turn the quantity check on or off. | Yes |
| **Quantity Must Be a Multiple of Minimum?** | When set to **Yes**, the quantity must be a multiple of the minimum value (e.g. minimum 6 allows 6, 12, 18, 24...). | Yes |
| **Quantity Restriction Applies To** | **Total Cart Quantity** checks the sum of all items in the cart. **Per-Item Quantity** checks each product line individually. | Total Cart Quantity |
| **Minimum Quantity** | The lowest quantity allowed. Set to `0` to skip the minimum check. | 0 |
| **Maximum Quantity** | The highest quantity allowed. Set to `0` to skip the maximum check. | 0 |

**Example:** Set **Quantity Restriction Applies To** to **Total Cart Quantity**, **Minimum Quantity** to `3`, and **Maximum Quantity** to `0` to require at least 3 items in the cart with no upper limit.

---

### Category-Based Settings tab

<!-- SCREENSHOT: Category-Based Settings fieldset in plugin configuration -->

This tab lets you require a minimum spend specifically on products from chosen Joomla content categories. This is useful when you sell a mix of product types and only want to enforce a minimum on a particular range.

| Field | Description |
|-------|-------------|
| **Restricted Categories** | Select one or more Joomla content categories. Products belonging to these categories (including their subcategories) count toward the category minimum. |
| **Minimum Category Order Value** | The combined value of items from the selected categories must reach this amount. If the shopper has qualifying products but their combined value is below this threshold, checkout is blocked. |

:::info

**Note:** The category restriction only triggers when a shopper's cart actually contains products from the selected categories. If their cart has no matching products, the check is skipped entirely.

:::

**Example:** Select your **Wholesale** category and set **Minimum Category Order Value** to `100` to require that wholesale items total at least $100 before checkout.

---

### Display tab

<!-- SCREENSHOT: Display fieldset in plugin configuration -->

| Field | Description | Default |
|-------|-------------|---------|
| **Template Framework** | Choose the front-end framework used to render restriction notices so the warning matches your site template. Select **Bootstrap 5** for Bootstrap-based templates or **UIkit** for UIkit-based templates. | Bootstrap 5 |

---

## Setting Per-Product Quantity Limits

In addition to the global rules above, you can override quantity limits on individual products. This is useful when a specific product must always be ordered in a set amount, regardless of global settings.

### Edit a Product

There are **three** ways to access products.

**Option A:** Go to the **J2Commerce** icon at the top right corner **-> Catalog -> Products**

**Option B:** Go to **Components** on the left sidebar **-> J2Commerce -> Products**

**Option C:** Go to **Content -> Categories ->** Find the category and then click inside the published article section

<!-- SCREENSHOT: Product list view -->

Open a product **-> J2Commerce** tab **-> Apps** tab. Locate the **Minimum Order Restriction** section.

<!-- SCREENSHOT: Product edit screen, Apps tab, Minimum Order Restriction section -->

### Per-Product Quantity Fields

| Field | Description | Default |
|-------|-------------|---------|
| **Enable Per-Product Quantity Restriction** | When enabled, this product uses its own minimum and maximum quantities and is excluded from the global per-item maximum. | No |
| **Product Minimum Quantity** | Minimum number of units of this product that must be in the cart at checkout. | 0 |
| **Product Maximum Quantity** | Maximum number of units of this product allowed in the cart at checkout. | 0 |

:::info

**Note:** Per-product limits take priority over the global quantity rules. When a product has its own restriction enabled, the global per-item maximum does not apply to that product.

:::

**Save** the product after making changes.

---

## How It Works

### At the cart drawer

When a shopper opens the cart drawer, J2Commerce checks the current cart against your configured rules. If any rule is violated, a warning banner appears next to the **Checkout** button and the button is disabled until the cart satisfies all restrictions.

### At checkout

When a shopper clicks **Checkout**, J2Commerce re-checks all rules on the server. If a violation is found, the shopper is redirected back to the cart page with a warning message describing which rule was not met. This server-side check is the definitive enforcement — even if a shopper bypasses the front-end warning, the checkout redirect still fires.

### Restriction evaluation order

The plugin checks rules in this sequence and stops at the first failing group:

1. **Price-Based** — Is the cart value high enough?
2. **Quantity-Based** — Are the quantity rules satisfied?
3. **Category-Based** — Is the category spend threshold met?

### Warning messages shoppers see

The exact text depends on which rule was violated:

- "Minimum order should be [amount] or above."
- "Total cart quantity should be [number] or above."
- "Total cart quantity should be [number] or below."
- "You have reached the minimum restriction for [product name] (minimum of [number])."
- "You have reached the maximum restriction for [product name] (maximum of [number])."
- "[Product name] must be ordered in multiples of [number]."
- "Total cart quantity must be a multiple of [number]."
- "Minimum category order value should be [amount] or above."

Multiple violations from the same check group are shown as a list.

---

## Tips

- **Start with the price-based rule** — a minimum order value is the most common requirement and the simplest to configure.
- **Use per-product limits for bundle products** — if a product is always sold in packs of 6, enable per-product restriction on that product and set minimum to `6` with **Quantity Must Be a Multiple of Minimum** to **Yes**.
- **Category restrictions complement global rules** — you can have both a global minimum cart value and a category-specific minimum active at the same time.
- **Match the template framework to your site** — choose Bootstrap 5 if your template is built on Bootstrap, or UIkit if you use a UIkit-based template, so warning banners look native.
- **Disable unused restriction types** — if you only need a minimum order value, turn off **Enable Quantity-Based Restriction** to avoid unexpected blocks.
- **Use debug mode during setup** — enable **Debug Mode** temporarily to confirm the plugin is evaluating orders correctly, then disable it before going live.

---

## Troubleshooting

### Shopper can still reach checkout despite a restriction being set

**Cause:** The app may be disabled, or the minimum value is set to `0`.

**Solution:**

1. Go to **J2Commerce -> Apps**.
2. Verify **Minimum Order Restriction** shows a green checkmark (enabled).
3. Open the app settings and confirm the relevant restriction section is set to **Yes**.
4. Check that the **Minimum Subtotal / Total** or **Minimum Quantity** field contains a value greater than `0`.

### Warning appears but the shopper can still click Checkout

**Cause:** The front-end cart drawer warning is informational only when JavaScript is disabled or blocked. The server-side redirect at checkout is the enforced check.

**Solution:** Verify that JavaScript is running in the shopper's browser. The checkout redirect will still fire on the server side regardless of whether the drawer warning rendered.

### Per-product limits are being ignored

**Cause:** The **Enable Per-Product Quantity Restriction** toggle is off on the product.

**Solution:**

1. Go to **J2Commerce -> Catalog -> Products** and open the product.
2. Go to the **J2Commerce** tab **-> Apps** tab **-> Minimum Order Restriction** section.
3. Set **Enable Per-Product Quantity Restriction** to **Yes**.
4. Enter the desired **Product Minimum Quantity** and/or **Product Maximum Quantity**.
5. Click **Save**.

### Category restriction never triggers

**Cause:** The cart contains no products from the selected categories, or no categories are selected.

**Solution:**

1. Open the app settings and go to **Category-Based Settings**.
2. Confirm that at least one category is selected in **Restricted Categories**.
3. Confirm **Minimum Category Order Value** is greater than `0`.
4. Verify the product's Joomla article belongs to one of the selected categories (or a subcategory of one).

### Warning banner style does not match the site

**Cause:** The **Template Framework** setting does not match the active site template.

**Solution:**

1. Open the app settings and go to **Display**.
2. Change **Template Framework** to match your site's front-end framework (**Bootstrap 5** or **UIkit**).
3. Click **Save**.
