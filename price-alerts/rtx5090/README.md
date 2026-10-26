# RTX 5090 price alerts (India)

A GitHub Actions job (`.github/workflows/rtx5090-price-alert.yml`) checks RTX 5090
listings every 3 hours, including out-of-stock ones, and opens a GitHub issue
that @mentions you (so GitHub emails it to you) when:

- an out-of-stock card comes back in stock (**BACK IN STOCK**, with its price), or
- an in-stock card drops below the last in-stock price seen for that listing
  (**PRICE DROP**).

Nothing is sent otherwise: no email for unchanged or higher prices.

It runs on GitHub's free servers: no PC, no email password, no Claude usage.

## Stores

| Store | How it's read | Reliability |
|---|---|---|
| Aashirwad Computers (SP Road, Bangalore, walk-in) | WooCommerce product API (JSON) | Good |
| Online King (Bangalore) | Store's own product API (JSON) | Good |
| NVIDIA Founders Edition (official India store) | WooCommerce product API (JSON) | Good |
| PrimeABGB | WooCommerce product API (JSON) | Blocks GitHub's servers at times |
| PC Studio | WooCommerce product API (JSON) | Blocks GitHub's servers at times |
| EZPZ Solutions | WooCommerce product API (JSON) | Good |
| EliteHubs | Shopify collection (JSON) | Good |
| Vishal Peripherals | Shopify collection (JSON) | Good |
| Vedant Computers | Search page HTML | Good |
| Computech Store | Search page HTML | Good |
| Flipkart | Search page HTML | OK, layout changes can break it |
| Amazon.in | Search page HTML | Best effort, often blocks cloud servers |

Not included because they block automated requests: MD Computers, ServerBasket,
GameLoot. Checked but no 5090 listings: TheITDepot, Clarion, PC Planet, TLG Gaming.
Listings below ₹2L or above ₹15L are ignored as placeholders, along with
laptops, prebuilt PCs and accessories. Tune these in `config.json`, where
each store can also be switched off.

To add another shop: if it runs WooCommerce or Shopify, it's one line in
`STORES` in `tracker.py` using the `woocommerce()` or `shopify_collection()` helper.

## Setup

Nothing to configure. Alerts arrive from notifications@github.com at the email
set under GitHub → Settings → Notifications → Default notifications email, as
long as email is ticked for "Participating, @mentions and custom".
Close the issues as you read them.

Optional repo variables (**Settings → Secrets and variables → Actions → Variables**):
- `ALERT_MENTION`: GitHub username to @mention, if not the repo owner.

To test, open **Actions → RTX 5090 price alert → Run workflow** and tick the
test box: it opens an issue with the current price list. The first scheduled run only saves a baseline.

## Run locally

```
pip install -r requirements.txt
SEND_SUMMARY=1 python tracker.py
```
