# 🦄 Analyzing Unicorn Companies
> **392 Companies · 33 Countries · $265B Total Valuation · 6 Global Regions**  
> *Crunchbase Emerging Unicorns Dataset — SQL + Excel + Statistical Analysis*

---

## 📌 Project Overview

This project explores **392 companies on the edge of unicorn status** 
($1B valuation) using SQL, Excel, and statistical hypothesis testing. 
The goal was to answer three business questions:

1. **Which countries and regions produce the most near-unicorns?**
2. **Does geography predict valuation size?**
3. **Does capital efficiency (valuation ÷ funding) predict final value?**

**Surprising answer to all three: No.** Valuations converge globally 
just below $1B regardless of where a company is headquartered, how much 
it raised, or how efficiently it deployed capital.

---

## 📁 Repository Contents

| File | Description |
|------|-------------|
| `emergingunicorn_companies.csv` | Raw dataset — 392 companies, 9 fields |
| `unicorn_companies_analysis.sql` | 18 SQL queries — full analysis script |
| `Unicorn_Companies_Dashboard.xlsx` | Excel dashboard — 5 sheets |

---

## 🗃️ Dataset

| Field | Description |
|-------|-------------|
| `company_name` | Company identifier |
| `country` | HQ country (33 unique) |
| `region` | Global region (6 unique) |
| `lead_investors` | Primary investor names |
| `post_money_value` | Post-money valuation (raw string) |
| `total_eq_funding` | Total equity raised (raw string) |

**After cleaning:** `valuation_m` and `funding_m` added as DECIMAL columns 
in $M, plus `efficiency_ratio` = valuation ÷ funding.

---

## 💾 SQL Analysis — 18 Queries Across 10 Concepts

```sql
-- Sample: Window function ranking within region
SELECT
    company_name, country, region, valuation_m,
    RANK()   OVER (PARTITION BY region ORDER BY valuation_m DESC) AS rank_in_region,
    NTILE(4) OVER (PARTITION BY region ORDER BY valuation_m DESC) AS quartile
FROM unicorn_companies
ORDER BY region, rank_in_region;
```

| Concept | Queries Covered |
|---------|----------------|
| Basic Aggregation | Region summary, country counts, global stats |
| `CASE` Statements | 5-tier capital efficiency classifier |
| `JOIN` | Company vs regional average, company vs country average |
| Window Functions | `RANK`, `DENSE_RANK`, `NTILE`, `LAG`, `LEAD` |
| CTEs | Above/below-average flagging with stddev bands |
| `GROUPING SETS` | Region + country + grand total in one query |
| Master Query | All metrics combined in a single CTE chain |

---

## 📊 Excel Dashboard — 5 Sheets

| Sheet | Contents |
|-------|----------|
| 🦄 Dashboard | KPI cards, top 10 countries, region breakdown, top 10 by valuation |
| 📋 Raw Data | All 392 records with cleaned valuation, funding & efficiency columns |
| 🧪 Statistical Tests | All hypothesis tests with Type I/II error framework |
| 📈 Charts Data | Bar chart + pie chart with live references |
| 💾 SQL Queries | All 7 key queries colour-coded for readability |

---

## 🧪 Statistical Testing Results

| Test | H₀ | Statistic | p-value | Decision |
|------|----|-----------|---------|----------|
| One-Sample T-Test | Mean valuation = $500M | t = 23.53 | < 0.001 | ✅ REJECT H₀ |
| T-Test: US vs India | μ_US = μ_India | t = -0.787 | 0.432 | ❌ FAIL TO REJECT |
| Z-Test: US vs China | μ_US = μ_China | Z = -0.878 | 0.380 | ❌ FAIL TO REJECT |
| One-Way ANOVA (3 Regions) | All region means equal | F = 1.478 | 0.229 | ❌ FAIL TO REJECT |
| T-Test: High vs Low Efficiency | Efficiency groups equal | t = 0.996 | 0.320 | ❌ FAIL TO REJECT |

**Error Framework:** α = 0.05 (Type I) · β = 0.20 (Type II) · Power = 0.80

---

## 💡 Key Insights

| # | Insight |
|---|---------|
| 1 | 🇺🇸 **US Monopoly by count** — 195/392 companies (49.7%), yet mean valuation is statistically equal to India and China |
| 2 | 📉 **Valuation convergence** — Mean ($676M) ≈ Median ($664M), an unusually symmetric distribution in financial data |
| 3 | 💰 **Capital efficiency is irrelevant** — Val/Fund ratio does not predict final valuation (p = 0.32) |
| 4 | 🌍 **Geography doesn't predict value** — ANOVA across 3 regions shows no significant effect (p = 0.23) |
| 5 | 🚀 **These are serious bets** — Mean funding of $186.9M is significantly above $100M (p < 0.001) |

---

## 🛠️ Tools & Skills


- **SQL:** JOINs, CTEs, Window Functions, GROUPING SETS, CASE, NULLIF
- **Excel:** KPI dashboards, bar/pie charts, conditional formatting
- **Statistics:** T-test, Z-test, One-Way ANOVA, Type I & II error framework
- **EDA:** Distribution analysis, outlier detection, efficiency ratios

---

## 👤 Author
**neel2444** · [GitHub](https://github.com/neel2444)
