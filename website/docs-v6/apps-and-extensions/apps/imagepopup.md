---
title: "Image Popup"
sidebar_label: "Image Popup"
sidebar_position: 40
description: "Configure the J2Commerce product image lightbox and add full-size image popups to category and tag list pages — no extra libraries required."
---

# Image Popup

The Image Popup app gives you fine-grained control over how product images open in a lightbox on your store. On product detail pages, J2Commerce already displays images in a Swiper carousel with a Fancybox lightbox built in. This app lets you tune how that lightbox behaves — looping, thumbnail strip, captions, and a slideshow button — and optionally extends the popup experience to category and tag list pages so shoppers can preview full-size images without navigating away from the listing.

No additional JavaScript or CSS libraries are installed. The app reuses the same Fancybox lightbox that J2Commerce already loads.

## Prerequisites

- J2Commerce 6 installed and active
- **Load Fancybox** enabled in J2Commerce configuration (it is on by default)
- The Image Popup add-on package downloaded from the [J2Commerce Extensions Store](https://www.j2commerce.com)

## Installation

This plugin is a separate add-on available from the [J2Commerce Extensions Store](https://www.j2commerce.com). It is not included with the core J2Commerce 6 component.

1. Purchase and download the `app_imagepopup.zip` package from the J2Commerce website.
2. Go to **System** -> **Install** -> **Extensions**.
3. Upload the `app_imagepopup.zip` package file.
4. The plugin installs automatically.

<!-- SCREENSHOT: System > Install > Extensions upload screen with app_imagepopup.zip selected -->

## Enabling the Plugin

After installation, enable the plugin before configuring it:

1. Go to **System** -> **Manage** -> **Extensions**.
2. In the **Search** field, type `Image Popup`.
3. Narrow the results by selecting **Plugin** in the **Type** filter if needed.
4. Click the red status icon in the row for **J2Commerce - Image Popup** to enable it (the icon turns green).

Alternatively, you can find and enable the plugin through the J2Commerce Apps panel:

1. Go to **J2Commerce** -> **Apps**.
2. Locate **Image Popup** in the list and click its status toggle to enable it.

<!-- SCREENSHOT: J2Commerce > Apps panel showing Image Popup in the list with its enable toggle -->

## Configuring the Plugin

Once the plugin is enabled, open its settings:

1. Go to **System** -> **Manage** -> **Plugins** (or **J2Commerce** -> **Apps**).
2. Click **J2Commerce - Image Popup** to open its settings.
3. Adjust the options in the **Basic Settings** tab (described in the table below).
4. Click **Save** to apply your changes.

<!-- SCREENSHOT: Image Popup plugin settings page showing all configuration fields -->

## Configuration Options

| Setting | Description | Default | Options |
|---------|-------------|---------|---------|
| **Show Popup in Product List View** | When enabled, clicking a product image in category and tag listing pages opens the full-size image in a popup. When disabled, clicking a product image in a list page navigates to the product detail page as normal. | No | Yes / No |
| **Loop Images** | After the last image in the popup, cycle back to the first image when the shopper clicks Next. | Yes | Yes / No |
| **Show Thumbnail Strip** | Display a row of small thumbnail images along the bottom of the popup so shoppers can jump directly to any image. | Yes | Yes / No |
| **Show Captions** | Display the product name as a caption beneath each image inside the popup. | Yes | Yes / No |
| **Show Slideshow Button** | Add a play button to the popup toolbar. When clicked, the popup automatically advances through all images at a set interval. | No | Yes / No |

### Recommended Starting Configuration

For most stores, the defaults work well. The one setting worth reviewing is **Show Popup in Product List View**: if your category pages use large hero-style product images and you want shoppers to linger on the listing rather than click through, set this to **Yes**.

## How It Works

### Product Detail Pages

On every product detail page, J2Commerce renders the product images inside a Swiper carousel. Each image already carries the Fancybox attributes the lightbox needs. When the Image Popup app is active, it passes your configured options (loop, thumbnails, captions, slideshow) to Fancybox so the lightbox behaves exactly as you have set it.

### Category and Tag List Pages

When **Show Popup in Product List View** is set to **Yes**, the app collects the main image and any additional images for each product card on the listing page. When a shopper clicks a product image, the popup opens and shows all of that product's images — the main image first, followed by additional images — rather than taking them to the product detail page.

Clicking anywhere outside the popup, or pressing Escape, closes it without affecting the listing page.

<!-- SCREENSHOT: Category list page with image popup open showing thumbnail strip and caption -->

## Checking the "Load Fancybox" Setting

The Image Popup app does not bundle any lightbox library. It relies on the Fancybox that J2Commerce already includes. If the popup does not open at all, the most likely cause is that Fancybox has been disabled in your store configuration.

To verify:

1. Go to **J2Commerce** -> **Configuration**.
2. Click the **Store** tab (or the **Layout** tab, depending on your version).
3. Look for **Load Fancybox** and confirm it is set to **Yes**.
4. Click **Save** if you changed anything.

<!-- SCREENSHOT: J2Commerce Configuration showing the Load Fancybox setting enabled -->

## What Is New Compared to J2Store

If you used the image popup feature in J2Store, here is what has changed in J2Commerce 6:

- **Single library, no choices.** The old app offered a drop-down to pick between PhotoSwipe, LightGallery, and Fancybox (jQuery). J2Commerce 6 uses only Fancybox v5 (vanilla JavaScript), so there is nothing to choose and nothing extra to load.
- **No jQuery dependency.** The popup is powered entirely by vanilla JavaScript. Pages load faster and there are no conflicts with other extensions that avoid jQuery.
- **Configurable from the plugin settings.** Loop, thumbnails, captions, and the slideshow button are all controlled through the plugin parameters — no template editing required.
- **List-view popups built in.** The ability to pop up images directly on category and tag listing pages is available as a single toggle, without needing a separate extension.

## Troubleshooting

### The popup does not open when I click a product image

**Cause:** Fancybox is not being loaded on the page.

**Solution:**

1. Go to **J2Commerce** -> **Configuration**.
2. Find the **Load Fancybox** setting and set it to **Yes**.
3. Click **Save** and reload your product page.

If the popup still does not open, check whether a caching plugin or CDN is serving a stale version of the page. Clear all caches (including Joomla's built-in cache via **System** -> **Clear Cache**) and try again.

### Images in list pages still navigate to the product page instead of opening a popup

**Cause:** **Show Popup in Product List View** is set to **No**, or the plugin is disabled.

**Solution:**

1. Go to the Image Popup plugin settings (see [Configuring the Plugin](#configuring-the-plugin) above).
2. Set **Show Popup in Product List View** to **Yes**.
3. Click **Save** and reload your category or tag listing page.
4. If the behaviour has not changed, confirm that the plugin is enabled (green status icon in **System** -> **Manage** -> **Extensions**).

### The popup opens but there are no thumbnails or captions

**Cause:** **Show Thumbnail Strip** or **Show Captions** is set to **No** in the plugin settings.

**Solution:** Open the Image Popup plugin settings, set both options to **Yes**, and click **Save**.

### My custom template does not show a popup on product detail pages

**Cause:** Some third-party Joomla templates override J2Commerce layout files and omit the Fancybox data attributes on product images.

**Solution:** Check your template's overrides for the J2Commerce product detail layout. The image elements need `data-fancybox="gallery"` attributes for the lightbox to attach. If those attributes are missing, restore the layout override from the original J2Commerce layout files or ask your template provider to add them.

## Related Topics

- [Apps and Extensions](../index.md)
- [J2Commerce Configuration](../../setup/index.md)
