# Debt Repayment Sweet Spot Calculator

A tool for identifying a mathematically derived weekly payment range for a consumer debt, requiring only the amount owed and the annual interest rate. No information about a consumer's income or ability to pay is needed.

Built for use by court observers, advocates, and neutral third parties facilitating debt collection negotiations.

---

## Methodology

### 1. Weekly Payoff Formula

Given a principal balance **P**, annual percentage rate **APR**, and weekly payment **m**, the number of weeks to pay off the debt is:

```
N = -log(1 - (P * r) / m) / log(1 + r)
```

where `r = APR / 52` is the weekly periodic rate. This is the discrete time loan amortization formula solved for the number of periods.

Any payment at or below `P * r` (the weekly interest-only amount) will never reduce the principal — the balance will remain flat or grow. This value is surfaced in the tool as the minimum viable payment threshold.

---

### 2. The Repayment Curve

Plotting weekly payment **m** on the x-axis against weeks to payoff **N** on the y-axis produces a strictly decreasing convex curve. The curve has a characteristic shape: steeply declining on the left where small payment increases save significant time, and nearly flat on the right where large additional payments yield diminishing returns.

The slope of this curve at any point **m** is approximated numerically as:

```
slope(m) = (N(m + 1) - N(m - 1)) / 2
```

This is a centered finite difference approximation of dN/dm evaluated at **m**, with a step size of $1.

---

### 3. Slope-Derived Payment Tiers

Three points on the curve are identified by solving for the payment **m** at which the slope equals a target value:

| Tier | Target Slope | Interpretation |
|---|---|---|
| Pay less, take longer | −2 | Last point where payments still produce strong reductions in payoff time |
| Sweet Spot | −1 | Inflection point — the mathematical elbow of the curve |
| Pay more, finish faster | −0.5 | Onset of clear diminishing returns |

The sweet spot at slope = −1 is the point where the marginal return on each additional dollar paid transitions from greater than one week saved per dollar to less than one week saved per dollar. It represents the optimal balance between repayment speed and payment burden without reference to any individual's financial circumstances.

Tier payments are rounded to the nearest $5 for practical use in negotiations: the lower tier rounds down, the sweet spot rounds to nearest, and the upper tier rounds up.

---

### 4. Check Your Number

Any proposed payment can be evaluated against the three tiers. The tool calculates the exact weeks to payoff and total interest paid at the proposed amount, and places it in context relative to the tiers — including the additional weeks and interest cost of paying below the sweet spot.

---

## Limitations

This tool makes no determination about a consumer's ability to pay. The sweet spot is a mathematically optimal payment given the debt terms alone — it is a starting point for a conversation, not a prescription. Actual payment plans should account for a consumer's full financial circumstances.

---

## Background

This tool is based on methodology originally described in [Debt collection lawsuits and payment plans](https://www.linkedin.com/pulse/debt-collection-lawsuits-payment-plans-lester-bird-dfwkc) (Lester Bird, 2024).
