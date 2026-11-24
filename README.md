# AIOD Discounts for Claude

Build, edit and troubleshoot the Shopify discounts that the **AIOD Discount & Gift** app runs on your store, by asking Claude in plain language.

- **Create offers:** BOGO and buy X get Y, free gifts with purchase, tiered and volume quantity breaks, bundles and fixed combo prices, free shipping thresholds, percentage or fixed-amount sales, and scheduled sales.
- **Edit offers:** raise a threshold, extend the dates, swap the gift, change the products, or change how an offer stacks with your other discounts.
- **Troubleshoot offers:** find out why a discount isn't applying, is applying the wrong amount, or isn't combining with another offer.

Requires the AIOD Discount & Gift app installed on your Shopify store.

## How it works

The plugin connects Claude to the AIOD connector and adds three skills that guide how Claude uses it:

| Skill | What Claude does |
|---|---|
| `create-discount` | Asks for anything missing, checks your other AIOD discounts for stacking conflicts, previews the offer as a draft for you to review, and creates it only when you ask. |
| `edit-discount` | Reads the current setup, previews the change as a draft, and warns you before overwriting a discount that is live for shoppers. |
| `troubleshoot-discount` | Checks status, dates and conditions against an example cart, compares stacking settings, and searches the AIOD setup guides. |

Anything Claude creates starts in **test mode**: it applies only to checkouts using your store's test email until you switch it to active in the AIOD app. Claude cannot activate or delete a discount.

## Data and privacy

The plugin contains no code. It configures one remote connector, `https://aiod-automatic-discount.com/mcp`, operated by AIOD. You sign in with your Shopify store, and Claude then calls it on your behalf.

- **Sent to AIOD:** the discount settings Claude reads, drafts, creates or changes, your search terms for the setup guides, and the wording of your request when Claude saves a draft.
- **Read from your store:** only the discounts the AIOD app manages, and aggregate performance figures for them (orders, revenue influenced, discount given).
- **Not accessed:** orders, customers, products beyond the ones a discount references, and discount codes or discounts created in the Shopify admin or by other apps.

AIOD keeps a usage log of connector calls (tool, outcome, timing) to run and support the service. Two of AIOD's service providers also receive data:

- **Mintlify**, which hosts AIOD's documentation, receives your search terms when Claude searches the setup guides.
- **PostHog**, AIOD's product-analytics provider, receives an event when Claude creates a discount: your store domain, the store's contact email and plan, and the kind of discount created.

Nothing is sent anywhere else. See the [privacy policy](https://www.aiodapp.com/privacy-policy/) and the [data processing terms](https://www.aiodapp.com/dpa/).

## Support

Contact AIOD at [aiodapp.com/contact](https://www.aiodapp.com/contact/) or aiod-app@dtcdevelopers.org.
