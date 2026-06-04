---
title: "Group Products"
sidebar_label: "Group Products"
sidebar_position: 1
description: "Let shoppers add multiple existing sub-products to their cart from a single product page using the Group Products type for J2Commerce 6."
---

# Group Products

Group Products lets you bundle a curated list of existing products under one parent listing. Shoppers see a table of sub-products on the product page — each with its own quantity field — and click **Add to Cart** to add every selected item to their cart as individual line items. The parent group product itself never appears in the cart.

This pattern is ideal for starter kits, equipment bundles, or "buy these together" listings where each item needs to remain a real, separately-priced cart line with its own shipping calculation, tax rule, and order tracking.

## Installation

This plugin is a separate add-on available from the [J2Commerce Extensions Store](https://www.j2commerce.com). It is not included with the core J2Commerce 6 component.

1. Purchase and download the `app_groupproducts.zip` package from the J2Commerce website.
2. Go to **System** -> **Install** -> **Extensions**.
3. Upload the `app_groupproducts.zip` package file.
4. The plugin installs and enables automatically.

After installation, configure the plugin at **J2Commerce** -> **Apps**.

<!-- SCREENSHOT: J2Commerce → Apps list with the Group Products row highlighted and its status toggle showing enabled -->

## What Changed from J2Store

If you used Group Products in J2Store v4, here is what is different in J2Commerce 6:

- **Native Joomla 6 MVC** — no FOF framework dependency. The plugin runs on namespaced classes and the standard Joomla event system.
- **No jQuery** — the admin sub-product picker and the front-end quantity-to-total recalculator are written in vanilla JavaScript (ES6+).
- **Sub-product list stored in product params** — the legacy `#__j2store_metafields` table is gone. The configured sub-products are now stored directly in the product's `params` field as a JSON array, which is more portable and does not require a separate database table.
- **Bootstrap 5 and UIkit templates** — Bootstrap 3 and Bootstrap 4 templates have been removed. The plugin ships with a Bootstrap 5 layout and a UIkit layout.
- **SKU search via standard Joomla AJAX** — the sub-product search in the admin editor now uses `com_ajax` (the standard Joomla AJAX dispatcher) instead of a custom FOF controller endpoint.

## Step-by-Step Setup

### Step 1: Enable the Plugin

1. Go to **J2Commerce** -> **Apps**.
2. Find **Group Products** in the list.
3. Click its status toggle to enable it if it is not already on.

<!-- SCREENSHOT: Apps list with Group Products row — status toggle in the enabled (green) state -->

### Step 2: Configure Global Settings

1. Still in **J2Commerce** -> **Apps**, click the **Group Products** name to open its settings.
2. Adjust the settings to match your store's requirements (see the [Settings Reference](#settings-reference) table below).
3. Click **Save** to apply.

<!-- SCREENSHOT: Group Products plugin settings panel — showing Default Quantity, Show Total Price, Show Checkbox, Show Price on Category Page, Template Type, and Debug Mode fields -->

### Step 3: Create a Group Product

1. Go to **J2Commerce** -> **Catalog** -> **Products**.
2. Click **New** in the toolbar.
3. Fill in the product **Title**, **Alias**, and any other general fields.
4. In the **General** tab, set **Product Type** to **Group Products**.

<!-- SCREENSHOT: Product edit form — General tab with Product Type dropdown open and "Group Products" highlighted -->

5. Click the **Group Products** tab that appears after setting the product type.
6. In the **Search Product** field, type at least two characters of a sub-product's SKU. A dropdown list of matching products appears.

<!-- SCREENSHOT: Product edit form — Group Products tab showing the search box with a partial SKU typed and a dropdown list of matching product names and SKUs -->

7. Click a product in the list to add it as a sub-product. It appears as a row in the table above the search box.
8. Repeat to add more sub-products.
9. To remove a sub-product, click the red trash icon on its row.

<!-- SCREENSHOT: Group Products tab showing two sub-products already added as rows in the table, each with a trash icon button -->

10. Finish filling in any other product tabs (pricing is not required for the parent product — leave the **Price** field at 0).
11. Click **Save & Close**.

> **Note:** Only Simple, Configurable, and Downloadable products that do not require option selection appear in the search results. Products with mandatory options (such as Variable products that require a variant choice) are excluded because they cannot be added to the cart silently in the background.

### Step 4: Verify the Frontend Display

Navigate to the product on your store. You should see a table listing the sub-products with columns for product name, SKU, price, and a quantity field.

<!-- SCREENSHOT: Frontend product page showing the grouped products table — columns for Product Name, SKU, Price, and Quantity with a numeric input per row, and the Add to Cart button below -->

When a shopper adjusts a quantity, the running total below the table updates automatically (if **Show Total Price** is enabled). Clicking **Add to Cart** adds each sub-product row to the cart as its own line item.

<!-- SCREENSHOT: Cart page after adding a group product — two sub-products appear as separate line items, each with their own quantity, price, and SKU -->

## Settings Reference

Access these settings at **J2Commerce** -> **Apps** -> **Group Products**.

| Setting | Description | Default | Notes |
|---------|-------------|---------|-------|
| **Default Quantity** | Pre-filled quantity shown in each sub-product's quantity input when the page loads. Set to `0` to show an empty field. | `0` | A value of `1` is the most common choice for kits where each item is needed once. |
| **Show Total Price** | Displays a running total price below the sub-products table. The total updates live as quantities change. | No | Recommended: **Yes** for any kit or bundle where price transparency matters. |
| **Show Checkbox** | Adds a checkbox column to the table. Shoppers must tick individual sub-products before clicking **Add to Cart**. Only checked items are added. | No | Use when the group is a "pick some" style offering rather than a fixed kit. |
| **Show Price on Category Page** | Shows a "Price starts from [lowest sub-product price]" label on the category listing page for group products. | No | Recommended: **Yes** when group products appear alongside other product types in a category. |
| **Template Type** | Controls which frontend template renders the sub-product table. **Auto** detects the active J2Commerce app template (Bootstrap 5 or UIkit) automatically. | Auto | Change to **Bootstrap 5** or **UIkit** only if automatic detection gives the wrong result. |
| **Debug Mode** | Writes plugin activity to `logs/app_groupproducts.php` in your Joomla logs directory. | No | Enable temporarily when diagnosing issues, then turn off in production. |

## Worked Example: Photography Starter Kit

Suppose you sell camera gear and want a single "Beginner DSLR Kit" listing that lets customers order a camera body, a memory card, and a camera bag in one step.

1. Ensure the camera body, memory card, and camera bag each exist as separate Simple products in your catalog with their own SKUs and prices.
2. Create a new product titled **Beginner DSLR Kit** and set **Product Type** to **Group Products**.
3. On the **Group Products** tab, search by SKU and add all three products.
4. Set the product price to `0` — the price on the parent is not shown or charged.
5. In the plugin settings, set **Default Quantity** to `1` and enable **Show Total Price**.
6. Publish the product.

On the frontend, shoppers see the three items in a table with a quantity of 1 each and a total price at the bottom. They can adjust quantities (for example, order two memory cards) and click **Add to Cart**. The camera body, memory card, and camera bag each become their own cart line — with individual tax calculations, shipping weights, and download links if applicable.

## Troubleshooting

### A product I added as a sub-product does not appear in the search results

**Cause:** The SKU search only returns Simple, Configurable, and Downloadable products that are published and visible. Products with required option selections (Variable, Flexi-Variable, Configurable with mandatory options) cannot be added silently to the cart and are intentionally excluded.

**Solution:** Open the product you want to use as a sub-product and verify that its **Product Type** is Simple, Configurable, or Downloadable. Also confirm it is published and that its **Visibility** is set to **Visible**.

---

### The running total does not update when I change a quantity

**Cause:** The **Show Total Price** setting is disabled in the plugin configuration.

**Solution:** Go to **J2Commerce** -> **Apps** -> **Group Products**, set **Show Total Price** to **Yes**, and save.

---

### Clicking Add to Cart does nothing when Show Checkbox is enabled

**Cause:** With **Show Checkbox** enabled, at least one sub-product must be ticked before the cart action fires. If no checkboxes are selected, the add-to-cart is blocked with a validation message.

**Solution:** Tick one or more sub-product checkboxes before clicking **Add to Cart**. If your product is meant to add all items without requiring a manual selection, disable **Show Checkbox** in the plugin settings.

---

### Sub-products appear in the cart as separate items instead of a single grouped item

**Cause:** This is the intended behaviour of Group Products. Each sub-product becomes its own cart line item so that it can be priced, taxed, and shipped independently.

**Solution:** If you need sub-products combined into a single cart entry with options, the [Box Builder Product](./app_boxbuilderproduct.md) add-on may suit your use case better.

---

### The group product table shows on the category page but no price label appears

**Cause:** The **Show Price on Category Page** setting is disabled.

**Solution:** Go to **J2Commerce** -> **Apps** -> **Group Products**, set **Show Price on Category Page** to **Yes**, and save. The label will read "Price starts from [lowest sub-product price]".

## Related Topics

- [Box Builder Product](./app_boxbuilderproduct.md) — adds sub-products as a single bundled cart item with options
- [Bundle Product](./app_bundleproduct.md) — fixed-quantity bundles with a combined price
- [Products Overview](../../catalog/index.md)
