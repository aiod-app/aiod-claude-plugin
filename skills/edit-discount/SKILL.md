---
name: edit-discount
description: Change an existing AIOD discount on a Shopify store — raise or lower a threshold, extend or end the dates, swap the gift, change the products, reshape a tier ladder, change how it stacks. Use when a merchant asks to update, adjust, extend or fix a discount that already exists in AIOD.
---

# Edit an AIOD discount

Edits go through the same preview-first flow as new discounts, with one extra risk: overwriting a live discount takes it out of service for real shoppers until it is reactivated.

## 1. Find the discount and read it

1. Call `shopify_discount_find` with a `query` from the merchant's words ("summer BOGO", "free tote") to get its `id`. If several match, list them and ask which one.
2. Call `shopify_discount_get` with that `id`. Note its `status`: an **active** discount is live for shoppers now.

## 2. Build the patch from the saved configuration

The `config` you send is a sparse patch merged onto the saved one:

- An omitted path keeps its saved value. A path you send replaces it, so `null`, `false`, `0`, `""` and `[]` are changes, not omissions.
- A condition, action or tier array you send replaces the whole array. To change one row, send every row, copying the unchanged ones from `shopify_discount_get`.
- The template can't change on an edit. If the merchant wants a different kind of offer, that is a new discount (see create-discount).
- Keep the stacking flags (`config.discount.product_discount`, `order_discount`, `shipping_discount`) unless the merchant asked to change how it combines.

When the change affects which products or dates it covers, or how it stacks, re-check the other discounts for conflicts as in create-discount, step 2.

## 3. Preview with a draft

Call `shopify_discount_draft` with `targetDiscountId` set to the discount's `id` and the patch as `config`.

- If it returns `issues`, fix the named paths and retry.
- `edit_changed_nothing` means the patch matches what's saved. Re-read the request rather than resubmitting.
- `template_field_out_of_scope` means this template doesn't support those fields. Tell the merchant that this setting isn't available for this kind of discount, and don't retry with those fields. Don't send them to the editor for it: some of these settings aren't in the AIOD editor either (for example, whether a shipping discount combines with other shipping discounts).
- Share the returned `url`. The saved discount is unchanged until the merchant saves the draft there.

## 4. Overwrite only when explicitly asked

`shopify_discount_update` writes the change to Shopify directly and puts the discount back into **test mode**. Use it only when the merchant explicitly wants the change applied now, not just previewed.

Before calling it on an **active** discount, say plainly: "This will take the discount off for real shoppers. It will only apply to the test email until you switch it back to active in AIOD." Wait for their go-ahead.

Afterwards, give them the `testModeEmail` to test with and the `url` where they reactivate it.
