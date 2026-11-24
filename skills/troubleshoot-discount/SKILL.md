---
name: troubleshoot-discount
description: Work out why an AIOD discount isn't applying, is applying wrongly, or clashes with another discount on a Shopify store — not showing in cart or checkout, wrong amount, two offers not stacking, gift not added. Use when a merchant reports a discount or promotion problem in AIOD.
---

# Troubleshoot an AIOD discount

Diagnose before changing anything. Most problems are a condition the cart doesn't meet, a stacking clash, or a storefront setup step.

## 1. Read the discount

1. `shopify_discount_find` → the discount's `id` and `status`.
   - **Test mode** applies only to checkouts using the store's test email, which is the most common "it's not working". The merchant activates it in the AIOD editor.
   - **Deactivated** status, or start and end dates that don't include today, means it isn't running.
2. `shopify_discount_get` → its conditions and rewards. Walk the merchant's example cart through them: products or collections, minimum subtotal or quantity, customer tags or segments, markets, purchase type (one-time vs subscription), dates.

Conditions inside one group must all hold. Separate groups are alternatives, and the first one that passes decides.

## 2. Check stacking conflicts

When the complaint is "only one discount applied" or "the discount disappeared when X was in the cart":

1. `shopify_discount_find` with `first: 50` (it defaults to the 10 newest) → other AIOD discounts that cover the same products or run at the same time. If `pageInfo.hasNextPage` is true, say that older discounts were not checked and ask the merchant about them.
2. `shopify_discount_get` on each → compare `config.discount.product_discount`, `order_discount` and `shipping_discount`. Two discounts only stack if each is set to combine with the other's class. Otherwise Shopify applies one.
3. Discount codes and automatic discounts from the Shopify admin or other apps are not visible to these tools but can still block or combine. Ask the merchant whether any are running.

## 3. Check storefront setup

For widgets not showing, gifts not being added, or behaviour in the cart, theme or POS, search the documentation with `shopify_discount_docs_search` (for example "gift not added to cart", "app embed", "combine with discount codes"), then read the most relevant page in full with its `page` value.

## 4. Propose the fix

Explain the cause in shopper terms, then fix it the edit-discount way: a draft with `targetDiscountId` for review, and `shopify_discount_update` only if the merchant explicitly asks to apply it now.
