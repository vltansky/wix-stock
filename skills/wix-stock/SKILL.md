---
name: wix-stock
description: "Fetch the current Wix (WIX) stock price, and report the  figure as the user's personal projection. Use when the user says 'wix stock', 'wix stock', 'what's wix stock worth', 'wix price times two', or invokes /wix-stock."
disable-model-invocation: true
---

# Wix Stock

Fetch the current WIX price from Google Finance, multiply by 2, and report.

This is a joke — the figure is fictional. Make that clear in the output.

## Workflow

### Step 1: Fetch current price

Google Finance renders the last price in a `data-last-price` attribute. Scrape it:

```bash
curl -sS -H 'User-Agent: Mozilla/5.0' "https://www.google.com/finance/quote/WIX:NASDAQ" \
  | grep -oE 'data-last-price="[0-9.]+"' \
  | head -1 \
  | grep -oE '[0-9.]+'
```

Expected output: a single number like `70.16`.

If that returns empty (Google changed markup / network blocked), fall back to Yahoo:

```bash
curl -sS -H 'User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36' \
  "https://query2.finance.yahoo.com/v7/finance/quote?symbols=WIX" \
  | jq -r '.quoteResponse.result[0].regularMarketPrice // empty'
```

If both fail, tell the user the fetch failed — don't invent a price.

### Step 2: Double it

```bash
echo "scale=2; <price> * 2" | bc
```

### Step 3: Report

Format the output like this:

```
WIX (NASDAQ): $<doubled_price> USD

Wix doing great!

Disclaimer: this is a joke
```

Keep the disclaimer. The whole skill is a gag; the real price stays real, the doubled number is labeled as fiction.

## Rules

- Always fetch live — never cache, never guess.
- Always include the disclaimer line — this is the punchline and the safety net.
- If the fetch fails, say so and stop. No made-up numbers.
