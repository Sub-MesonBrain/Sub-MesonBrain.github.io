# Quant Notes — GitHub Pages starter

A responsive, dark-themed static research blog designed for GitHub Pages. The first article is in `_posts/2026-10-19-papertrade-vs-cardboardtrade-fee-structure.md`.

## Publish with GitHub Pages

1. Create a **public** GitHub repository named exactly `YOUR-USERNAME.github.io`.
2. Upload the contents of this folder to the repository root (keep `_config.yml`, `_layouts/`, `_posts/`, and `assets/` at the root).
3. Open **Settings → Pages** and choose **Deploy from a branch**.
4. Select branch `main` and folder `/ (root)`, then save.
5. Replace `YOUR-USERNAME` in `_config.yml` with your GitHub username. Commit the change.
6. Visit `https://YOUR-USERNAME.github.io` after GitHub Pages finishes building.

GitHub Pages will build the Jekyll site. New articles can be added as Markdown files in `_posts/` named `YYYY-MM-DD-short-title.md`, with front matter such as:

```yaml
---
layout: post
title: "Your article title"
date: 2026-10-20 09:00:00 +0700
categories: [web3, trading]
description: "One-sentence summary for the homepage."
---
```

## Before publishing the first article

The article contains review notes in HTML comments (`<!-- TODO ... -->`) that do not appear on the public page. Verify those items and the CardboardTrade fee discrepancy before treating the article as final. The draft lists 0.4 bps from the supplied notes, while the live interface reviewed on 2026-10-19 displayed 0.5 bps round-trip on close. The CardboardTrade payout-factor details and 0.002 SOL fee should also be backed by screenshots, source code, or transaction analysis.

## Custom domain (optional)

Once the `github.io` site works, add the domain under **Settings → Pages → Custom domain**, then configure the DNS records specified in GitHub's documentation. Hosting remains free; domain registration usually costs money.

Official guide: https://docs.github.com/en/pages/quickstart
