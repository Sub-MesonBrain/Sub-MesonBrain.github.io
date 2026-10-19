---
layout: post
title: "PaperTrade vs. CardboardTrade: A Fee Structure Comparison"
date: 2026-10-19 09:00:00 +0700
categories: [web3, trading]
tags: [perpetuals, fee-analysis, crypto, draft-review]
description: Beyond headline fees, winning-PnL adjustments, liquidation buffers, payout rules, and transaction costs can change how much a trader keeps.
reading_time: "8 min read"
---

A comparison of trading venues should look beyond deposit and withdrawal fees. Winning-PnL adjustments, liquidation-price buffers, explicit closing charges, and chain transaction costs can all affect the final result.

This note compares the mechanisms described in the source material for **PaperTrade** and **CardboardTrade**. Fee settings and interfaces can change; check the live interface and transaction-level behavior before using these figures to make a trading decision.

## 1. PaperTrade — Winning PnL reduction

PaperTrade reduces winning realized PnL through a **0.2 bps deadband**, a dynamic impact adjustment, and a separate **2% win fee** described in the published mechanism analysis.

### Deadband: 0.2 bps

When closing a winning position, the first 0.2 basis points of favorable price movement are removed before the impact curve is applied. In price terms, that is approximately `entry price / 50,000`.

The impact grows with leverage when measured against margin. At **1,000× leverage, a 0.2 bps price adjustment is equivalent to about 2% of the margin**.

### Dynamic impact adjustment

The impact curve is parameterized by several values:

- `baseRate = 10%`: the adjustment curve approaches a 10% base haircut as the favorable price move becomes large.
- `rateMultiplier = 15,000`: a smaller value produces a stronger reduction for a given move.
- `referenceNotional / positionMultiplier`: this market-specific term affects the curve most strongly for smaller price moves; its effect diminishes as the move grows.

The mechanism analysis reports that, for BTC, a 1% favorable price move retains approximately **88.1% of raw gains before the separate 2% win fee**, while a 0.1% move retains approximately **73.9% before that fee**. After applying a 2% fee to those adjusted payouts, the approximate retained shares are **86.3%** and **72.4%**, respectively. These are model-derived estimates, not a guarantee of realized outcomes.

<!-- TODO before final publication: attach the PaperTrade impact-adjustment screenshot and verify the parameters against the latest official source. -->

## 2. PaperTrade — Liquidation-price adjustment

The liquidation threshold is adjusted by approximately **5 bps in the unfavorable direction**. Because this buffer is measured against price rather than margin, its effect becomes more significant as leverage increases.

For example, at **1,000× leverage**, a 5 bps price buffer is roughly half of the 0.1% move that would otherwise exhaust the margin, leaving only about 0.05 percentage points of price movement between the adjusted threshold and that reference point.

## 3. PaperTrade — Losses

A losing close does not incur an additional trader-side charge on top of the trading loss in the mechanism described here. Any LP-side fee is a separate accounting item and should not be described as an extra fee charged to the losing trader.

## 4. CardboardTrade — Winning PnL reduction

The source notes for CardboardTrade describe several costs and payout rules:

- **Profit deadband:** 0.2 bps.
- **Payout factor:** reported as 100% for smaller positions, falling to a 30% cap for larger positions.
- **Position sizes without a payout cutoff:** BTC up to $9,000,000; ETH up to $3,375,000; SOL up to $1,500,000, according to the source notes used for this comparison.
- **Profit fee:** reported as 2%.
- **Closing fee:** the original source notes list 0.4 bps.

The payout factor is especially important for larger positions because it can reduce the amount of profit paid to the trader.

<!-- TODO before final publication: attach the CardboardTrade payout-factor screenshot and verify the payout caps, size thresholds, profit fee, and closing fee against the latest UI/contract behavior. The live interface reviewed on 2026-10-19 displayed a 0.5 bps round-trip fee on close, which conflicts with the 0.4 bps value in the source notes. -->

## 5. CardboardTrade — Liquidation-price adjustment

The source notes describe a **5 bps unfavorable liquidation-price adjustment**, similar to PaperTrade. At high leverage, even a small price buffer can materially reduce the available room before liquidation.

## 6. CardboardTrade — Losses

The source notes indicate that no additional trader-side fee is charged on top of the trading loss. This should be distinguished from any platform-side or liquidity-provider accounting fees.

## 7. The extra cost — Solana transaction fees

Unlike PaperTrade, which operates on HyperEVM, CardboardTrade's source notes list a platform-managed Solana transaction fee of **0.002 SOL per trade**.

For high-frequency trading, a per-trade cost can accumulate quickly. Its USD value also changes with SOL's market price. Confirm this amount from recent transactions or the current official interface before publication.

## 8. Side-by-side comparison

| Fee or mechanism | PaperTrade | CardboardTrade |
| --- | --- | --- |
| Profit deadband | 0.2 bps | 0.2 bps |
| Winning-PnL adjustment | Dynamic impact curve | Payout-factor mechanism (per source notes) |
| Profit / win fee | Separate 2% win fee in the published mechanism analysis | 2% in the source notes; verify current implementation |
| Closing fee | No separate closing fee listed in the source material reviewed here | 0.4 bps in source notes; live UI reviewed 2026-10-19 showed 0.5 bps round-trip on close |
| Liquidation adjustment | Approximately 5 bps unfavorable | Approximately 5 bps unfavorable per source notes |
| Additional trader-side fee on losses | None described | None described in source notes |
| Chain transaction fee | No separate Solana transaction fee | 0.002 SOL per trade in source notes; verify current behavior |

## Bottom line

Headline fees tell only part of the story. Winning-PnL adjustments can reduce profits, liquidation-price buffers can matter disproportionately at high leverage, and explicit closing or transaction fees can compound for active traders.

The useful comparison is not just the stated fee rate. It is **how much of the raw PnL remains after all adjustments**, under the same asset, move size, position size, leverage, and execution assumptions.

### Sources and scope

- [PaperTrade](https://papertrade.xyz/) — official trading interface and current settings.
- [CardboardTrade](https://cardboardtrade.org/) — official trading interface and current settings.
- [How Papertrade has built an amazing product on HyperEVM](https://uditsamani.com/papertrade-hyperevm/) — independent mechanism analysis covering the deadband, impact curve, win fee, and liquidation buffer.

**Research note:** This article is a mechanism comparison, not investment advice. Some CardboardTrade values above come from source notes supplied for the comparison and require re-verification against current UI behavior or transaction data. The platforms may change their parameters over time.
