---
name: create-discount
description: Create a new Shopify discount or promotion with the AIOD app — BOGO / buy X get Y, free gift with purchase, tiered or volume breaks, bundles and fixed combo prices, free shipping thresholds, percentage or fixed-amount sales, scheduled sales. Use when a merchant asks to set up, build, launch or try out a new offer in their AIOD store.
---

# Create a discount with AIOD

The AIOD tools (`shopify_discount_*`) build discounts that the AIOD app runs on the merchant's Shopify store. Work in this order: understand, check for conflicts, preview, then create only when asked.

## 1. Pin down the offer

Before building anything, make sure you know:

- **What qualifies:** which products, variants or collections; a minimum cart subtotal or item quantity; customer tags, segments or markets, if any.
- **What it gives:** a percentage or fixed amount off, a free or discounted gift, free or discounted shipping, or a tier ladder.
- **When it runs:** start and end dates, or no end date.
- **Whether it should stack** with the store's other discounts (see step 2).

Ask one short question for anything missing rather than guessing a product, threshold or date. Products and collections are referenced by Shopify GID; ask the merchant which ones when they are vague ("my summer stuff").

## 2. Check for stacking conflicts

New offers most often misbehave because of how they combine with what is already running.

1. Call `shopify_discount_find` with `first: 50` to list the store's discounts. It returns the newest first and defaults to only 10. If `pageInfo.hasNextPage` is still true, tell the merchant that only the 50 most recent were checked, and ask about older discounts on the same products. Those with status `active` or `test-mode` are the ones that can interact.
2. For any that target the same products or collections, or overlap in dates, call `shopify_discount_get` and read its `config.discount` flags: `product_discount`, `order_discount` and `shipping_discount` say which other discount classes it combines with.
3. Tell the merchant plainly about any overlap, for example: "Your Summer BOGO also covers these tees and doesn't combine with other product discounts, so customers will get whichever Shopify picks as better, not both." Then ask whether the new offer should stack, replace or exclude.

`shopify_discount_find` only sees discounts AIOD manages. Mention that discount codes and automatic discounts created in the Shopify admin or another app can also interact and are not visible here.

## 3. Read the schema, then preview with a draft

1. Call `shopify_discount_config_schema` (section `contracts_1`, then `contracts_2` if the template isn't there) to choose the `template` and see its editable paths. Use `conditions` and `actions_and_rules` for exact field syntax, and `examples_1` / `examples_2` for worked configurations.
2. Call `shopify_discount_draft` with a sparse `config` (only the fields being set) and the merchant's request as `userMessage`.
3. If it returns `issues`, fix the named paths and retry. Report any `ignoredPaths` (fields the template doesn't allow) and any `warnings` to the merchant rather than silently dropping them.
4. Share the returned `url`. It opens the AIOD editor prefilled with the offer, where the merchant can review and save. Nothing exists in Shopify until they do.

Default to the draft. It is the safe preview and lets the merchant adjust details in the editor's pickers.

## 4. Create in test mode only when asked

Use `shopify_discount_publish` only when the merchant asks for the discount to be created, or wants to try it on the live storefront.

- It creates a real Shopify discount in **test mode**: it applies only to checkouts using the returned `testModeEmail`, so real shoppers are unaffected.
- Give the merchant the `testModeEmail` and the returned `url`, and tell them how to test it: check out with that email. To go live, they switch it to active in the AIOD editor. No tool can activate or delete a discount.
- `shopify_discount_publish` only creates. To change an existing discount, use the edit-discount workflow.

## Writing style for the merchant

Describe offers in shopper terms ("Buy 2 tees, get the 3rd free, through Nov 30"), not configuration JSON. Summarise what was created, where to review it, and anything that was not applied.
