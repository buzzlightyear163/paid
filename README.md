# PAID ($PAID)

> buys the second the dev pays.

This is a single-file site (`index.html`, with the CSS and JS baked in), plus `og.png` and `fav.png`. It uses the termkit **paper** theme. The pixel object is a rubber stamp (`TK.OBJECTS.stamp`) lifted off a fresh green PAID imprint, with a check mark on its label.

## Fill in CONFIG

CONFIG is at the top of the last `<script>` in index.html.

| field | what it does |
|---|---|
| `ca` | The $PAID mint after launch. Left empty, the site shows "posted here and on X at launch". Once filled in, it turns on the copy button and the buy links (pump.fun). |
| `x` | The X profile URL. |
| `site` | The site's own domain, for example `https://paid.fun`. It is printed on the receipt card and added to the X share text. |
| `dexscreener` | Optional link to $PAID's own DexScreener page. |
| `pollMs` | How often the feed polls. The default is 3000. It cycles through the four feeds, so each feed is refreshed every 12 s. |

## What is real

- **Live feed (02):** DexScreener's public API with no key, read straight from the visitor's browser:
  - `token-profiles/latest/v1` gives dex paid.
  - `token-boosts/latest/v1`, `community-takeovers/latest/v1` and `ads/latest/v1` give the other signals.
  - `tokens/v1/solana/<up to 30 mints>` gives symbol, mcap, liquidity and the earliest pair, which counts as the launch.
  - `orders/v1/solana/<mint>` gives what the dev paid for and `paymentTimestamp`. This is where **launch→paid** comes from.
  - Rate limits (60/min on profiles and orders) are respected, and a 429 pauses polling for 20 s.
- **Stats:** coins on the feed, median launch→paid, fastest dev and caught live. They are computed from what the visitor has seen during the session.
- **Check (03):** paste a mint to see dex paid ✓ or not, every paid order (type, status, time), launch→paid, mcap and liquidity. It draws a 1200×630 receipt card that can be saved as a PNG or posted with an X intent.
- **Preview:** if DexScreener can't be reached, rows are shown tagged `preview` and the lamp says `preview`. The moment real data arrives, the lamp switches to `live · dexscreener` and the preview rows are removed.

## What is larp (marked beta)

- **The desk (04):** it runs the visitor's rules on the real feed and shows "would buy" or "skip · reason". The rules are signals, size, max mcap, min liquidity, paid within X min of launch, and pump.fun only. "Armed" executes nothing, connects no wallet and moves no funds. Settings are saved locally (localStorage).

## Deploy

GitHub → Vercel. Set Framework to **Other**, with no build command, and output = repo root. Put `index.html`, `og.png` and `fav.png` in the root.

## Check after deploy

1. **Fonts:** JetBrains Mono loads (Google Fonts).
2. **X preview:** paste the URL into a draft post. If the card has no image, change `og:image` and `twitter:image` to the full URL (`https://yourdomain/og.png`).
3. **Live data:** the lamp in 02 should switch to `live · dexscreener` within a few seconds and the feed should fill with Solana coins. If it stays on `preview`, open the console: a CORS error means DexScreener is blocking the domain, and the API calls would then need to go through a small Vercel function.
4. **Check:** paste the mint of a coin you know is dex paid and confirm that ✓, the time and launch→paid look right against DexScreener.
5. **After launch:** set `ca` and confirm the buy links go to the right coin.
