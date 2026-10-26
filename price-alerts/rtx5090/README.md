# RTX 5090 price alerts (India)

A GitHub Actions job (`.github/workflows/rtx5090-price-alert.yml`) checks RTX 5090
listings every 3 hours and emails you when an in-stock card:

- gets cheaper (**PRICE DROP**)
- comes back in stock (**BACK IN STOCK**)
- appears for the first time (**NEW LISTING**)
- falls to or below your target price (**BELOW TARGET**)

It runs on GitHub's servers, so your PC can be off.

## Stores

| Store | How it's read | Reliability |
|---|---|---|
| Aashirwad Computers (SP Road, Bangalore, walk-in) | WooCommerce product API (JSON) | Good |
| Online King (Bangalore) | Store's own product API (JSON) | Good |
| PrimeABGB | WooCommerce product API (JSON) | Good |
| EliteHubs | Shopify collection (JSON) | Good |
| Vishal Peripherals | Shopify collection (JSON) | Good |
| Vedant Computers | Search page HTML | Good |
| Flipkart | Search page HTML | OK, layout changes can break it |
| Amazon.in | Search page HTML | Best effort, often blocks cloud servers |

MD Computers is not included: it blocks automated requests (Cloudflare).
Listings below ₹2L or above ₹15L are ignored as placeholders, along with
laptops, prebuilt PCs and accessories. Tune these in `config.json`, where
each store can also be switched off.

To add another shop: if it runs WooCommerce or Shopify, it's one line in
`STORES` in `tracker.py` using the `woocommerce()` or `shopify_collection()` helper.

## Setup

In the repo, go to **Settings → Secrets and variables → Actions**.

Secrets:
- `GMAIL_ADDRESS`: the Gmail account that sends the alerts.
- `GMAIL_APP_PASSWORD`: an App Password for that account
  (Google Account → Security → 2-Step Verification → App passwords).

Variables (optional):
- `TARGET_PRICE`: e.g. `250000` to also get an alert at or below ₹2,50,000.
- `ALERT_TO`: comma-separated recipients, if not the sending address.

Then open **Actions → RTX 5090 price alert → Run workflow** to get a test email
with the current price list. The first scheduled run only saves a baseline.

## Run locally

```
pip install -r requirements.txt
DRY_RUN=1 SEND_SUMMARY=1 python tracker.py
```
