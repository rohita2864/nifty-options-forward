<div align="center">

# NIFTY options: a systematic forward test

**An automated options-selling system, run live every trading day, with every decision fingerprinted in public before
the market closes.**

| 18 of 19 | 2.02 · 2.67 | −7.1% | Live |
|:---:|:---:|:---:|:---:|
| years positive, 2008–2026 | Sharpe, development · held out | worst drawdown since 2008 | since 5 Nov 2026 |

</div>

## Live

<!-- live:start -->
| Live since | Days valued | Account (paper, Rs) | Return since go-live | Last result | Latest fingerprint |
|---|---|---|---|---|---|
| 05 Nov 2026 | 0 | 10,000,000 | +0.00% | first result tomorrow | `ef107d7342b7...` decided 2026-11-05 at 15:14:07 IST |

*Updated automatically by each day's run. Failed days: 0. Every row: [records/daily.csv](records/daily.csv).*
<!-- live:end -->

![backtest](records/backtest.png)

## How it works

```mermaid
flowchart LR
    A([Market data]) --> B([Model]) --> C([Option engine]) --> D([Optimizer]) --> E([Today's book])
    classDef s fill:#f4f6f8,stroke:#9aa5b1,color:#1f2933
    class A,B,C,D,E s
```

Every trading day at about 15:15 IST the system decides which NIFTY options to sell, publishes a fingerprint of that
decision here before the 15:30 close, and reports the result the next day. It runs on its own: nothing is changed by
hand, and nothing in it learns from results. **Frozen since 5 November 2026**; any change will be announced here first.

## Results

A ₹1 crore paper account, run day by day with realistic costs and fills. Before interest on the cash.

| Period | Return / yr | Sharpe | Max drawdown |
|---|---|---|---|
| 2008 – 2022 · development | +7.7% | 2.02 | −7.1% |
| 2023 – Oct 2026 · held out | +11.7% | 2.67 | −5.2% |
| Oct – Nov 2026 · fresh data, system frozen | +1.32% in 18 days | – | −0.2% |
| From 5 Nov 2026 · live | see above | | |

<details>
<summary><b>Year by year</b></summary>

| Year | Return | Sharpe | Max drawdown |
|---|---|---|---|
| 2008 | +2.0% | 0.49 | −3.9% |
| 2009 | +7.5% | 2.85 | −1.9% |
| 2010 | +11.8% | 3.10 | −3.3% |
| 2011 | +5.8% | 1.20 | −3.8% |
| 2012 | +11.3% | 2.96 | −1.5% |
| 2013 | +2.4% | 0.57 | −5.8% |
| 2014 | −0.4% | -0.04 | −7.1% |
| 2015 | +8.5% | 3.08 | −1.6% |
| 2016 | +7.8% | 2.73 | −1.4% |
| 2017 | +3.4% | 1.76 | −1.0% |
| 2018 | +6.6% | 2.27 | −1.3% |
| 2019 | +8.5% | 2.24 | −3.3% |
| 2020 | +14.8% | 3.44 | −3.3% |
| 2021 | +12.8% | 5.00 | −0.8% |
| 2022 | +13.4% | 3.07 | −2.1% |
| 2023 · | +8.5% | 4.34 | −1.1% |
| 2024 · | +12.9% | 3.64 | −1.1% |
| 2025 · | +10.8% | 1.63 | −5.2% |
| 2026 · | +13.4% | 4.21 | −1.4% |

· held out. 2026 to 2 November.
</details>

<details>
<summary><b>Monthly returns</b></summary>

![monthly returns](records/monthly.png)
</details>

<details>
<summary><b>On the market's worst days</b></summary>

| Day | Event | NIFTY | Account |
|---|---|---|---|
| 2008-01-21 | Global sell-off | −8.7% | −2.58% |
| 2008-10-24 | Financial crisis | −12.2% | −0.03% |
| 2011-09-22 | Euro-debt sell-off | −4.1% | −3.66% |
| 2015-08-24 | China devaluation crash | −5.9% | −0.13% (21–25 Aug) |
| 2016-11-09 | Demonetisation + US election | −1.3% | +0.27% |
| 2019-09-20 | Corporate tax cut (+5%) | +5.3% | −2.01% |
| 2020-03-12 | COVID crash | −8.3% | −1.95% |
| 2020-03-23 | COVID crash, lower circuit day | −13.0% | −1.04% |
| 2022-02-24 | Russia invades Ukraine | −4.8% | −2.09% |
| 2024-06-04 | Election results | −5.9% | −0.01% |
| 2025-04-07 | US tariff crash | −3.2% | −5.11% |
| COVID crash, 20 Feb - 23 Mar 2020 | | −37.2% | −3.12% |
| Financial crisis, Sep - Nov 2008 | | −36.8% | −0.81% |

Losses are shown as they happened. Sudden large moves are the risk of selling options, and they are in every number
above.
</details>

## Verify it yourself

```mermaid
flowchart LR
    A([Book decided]) --> B([Fingerprint published]) --> C([Market closes]) --> D([Result published])
    classDef s fill:#f4f6f8,stroke:#9aa5b1,color:#1f2933
    class A,B,C,D s
```

Each day's book gets a SHA-256 fingerprint, pushed here before the close. GitHub stamps every push with its own server
time (see the repository's **Activity** page), so nobody, including me, can change a decision after seeing the outcome.
The history is never rewritten. Any day's book can be shown privately and checked against its fingerprint.

<details>
<summary><b>Why positions are not published</b></summary>

So that the method cannot be copied or learned from its outputs. The fingerprints prove each day's book was fixed in
advance; a book carries a random nonce, so its fingerprint cannot be matched by guessing.
</details>

<details>
<summary><b>What is counted, and the limits</b></summary>

- **Costs:** quoted bid / ask spreads, exchange and SEBI charges, STT, stamp duty, GST, hedging costs. Brokerage is not
  included (a firm trading on its own exchange membership pays none).
- **Fills** are limited to a share of each option's traded volume; margin is checked every day.
- **Every day counted**, including days the account could not be valued, which are compounded across.
- A paper account, not executed with a broker. Backtest on NSE closing prices; live on NSE quotes at about 15:15.
- A run that fails (data or network) is retried every 2 minutes, up to 5 times, and otherwise shown as failed.
- NSE publishes no F&O file for two trading days (9 Oct 2013, 30 Mar 2021); the backtest values across them.
</details>

<details>
<summary><b>The daily record</b></summary>

`records/daily.csv`: one row per trading day with the decision date, fingerprint, valuation date, result after costs,
trading costs, share of capital used as margin, account value, and status (decided, valued, run_failed, value_failed).
</details>

---

<sub>A research record of a paper-traded system, not investment advice.</sub>
