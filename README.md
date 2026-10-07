# 📊 Financial Performance Analysis (Power BI)

An end-to-end financial performance report built in Power BI on a **Business Controller case study** dataset, focused on **KPI monitoring, cost structure analysis, and management-level insights** — comparing Actual results against Previous Year and Budget across MTD, QTD, and YTD views.

`Power BI` · `PBIP / TMDL` · `DAX` · `Calculation Groups` · `Power Query` · `Financial KPI Modeling`

---

## 🎯 Business Goal

Management needs to quickly see where financial performance is deviating from plan — and why — without digging through raw ledger exports. The report turns monthly actuals from accounting and the controlling budget into a P&L (Revenue → COGS → Gross Profit → Variable Costs → Contribution Profit → OPEX → EBITDA) and decision-ready KPIs (margins, cost-to-revenue ratios) compared against Previous Year and Budget, so leadership can spot underperforming periods and cost drivers at a glance.

## 📷 Report Preview

<img width="1107" height="627" alt="image" src="https://github.com/user-attachments/assets/3b6f1dfb-3934-401d-a387-fb4286252052" />
<img width="1108" height="630" alt="image" src="https://github.com/user-attachments/assets/371a2d6f-1559-403e-a71d-ce0e4e857883" />


## 📊 Key Analytical Areas

| Area | Focus |
|---|---|
| **KPI Analysis** | Gross Profit Margin, Contribution Margin, EBITDA Margin, cost-to-revenue ratios (COGS, Variable Costs, OPEX) |
| **Cost Structure** | Variable costs (Warehouse, Distribution, Payment), OPEX (Google vs. Meta marketing spend), cost rigidity vs. revenue decline |
| **Trend Analysis** | Quarterly comparisons, monthly deep-dives, identifying critical periods (e.g. April) |

Every view can be switched between **MTD / QTD / YTD** and compared against **PY** or **Budget**.

## 🏗️ Data Model

| Table | Role | Grain |
|---|---|---|
| `Actuals` | Fact | Account × month (posting lines only) |
| `Budget` | Fact | Cost group (P&L line / Type1) × month |
| `Account` | Dimension | Account, incl. P&L line, cost group, market and the subtotal lines |
| `Date` | Date dimension (`CALENDARAUTO`) | Day |
| `Comparison` | Calculation group | Actual / PY / Budget / Previous Period / Var % items |
| slicer & field-parameter tables | UI | `Period Type`, `Benchmark`, `Amount KPI`, `Margin KPI`, `Cost Ratio KPI`, `Param *` |

<details>
<summary><b>🧠 Key design decisions (click to expand)</b></summary>

- **Subtotals are calculated, not stored.** The source sheet contains Gross Profit, Contribution Profit and EBITDA as data rows. They are removed in Power Query; the `Account` dimension keeps them as statement lines and the `[Actuals]` measure calculates them (one line → its amount, several lines or the grand total → net result = revenue − costs). Totals in a matrix are therefore always meaningful.
- **Natural keys.** Actuals join the `Account` dimension on the general-ledger **account number**, not on concatenated names.
- **Budget at a different grain.** The budget exists per cost group only, so `Budget` has no physical relationship to `Account`. It is filtered through a virtual relationship (`TREATAS` on `Group Key`) and is blank below cost-group level (account, market) instead of repeating the group total — no bidirectional relationships in the model.
- **Calculation group instead of copy-paste.** PY, Budget, previous-period and variance logic lives once in the `Comparison` calculation group; every margin / ratio `PY` and `Budget` measure is a one-line `CALCULATE` applying an item. Each item removes its own filter, so measures that call other measures are not shifted twice.
- **Market attribute.** The market (DE / PL / NL / Others) is extracted from revenue and COGS account names into its own column; other cost lines are "Not allocated".
- **Readable model.** Descriptive table, column and measure names, display folders, descriptions on every table and key measure, a single `SourceFile` parameter for the data path, and the PBIP / TMDL format so every change is visible in Git.

</details>

## ✅ Key Findings

- Overall H1 performance is **negative vs. both PY and Budget**
- Slight improvement only in Gross Profit Margin YTD and COGS Ratio YTD
- Other profitability metrics deteriorated — driven by revenue decline combined with insufficient cost reduction in Variable Costs and OPEX
- Contribution Margin and EBITDA show significant pressure on operational profitability

Full written analysis: [`Analysis_Conclusions.docx`](Analysis_Conclusions.docx)

## ▶️ How to Run

1. Clone the repo and open `Financial_Performance_Analysis.pbip` in Power BI Desktop.
2. **Transform data → Edit parameters** → set `SourceFile` to the full path of `Source_Financial_Data.xlsx` in your clone.
3. **Refresh**.

## 📂 Repository Structure

```
Financial_Performance_Analysis.pbip            # open this in Power BI Desktop
Financial_Performance_Analysis.Report/         # report (pages, visuals)
Financial_Performance_Analysis.SemanticModel/  # model in TMDL: tables, measures, calculation group
Source_Financial_Data.xlsx                     # case-study data (Actuals, Budget)
Analysis_Conclusions.docx                      # written management summary
```

## 💡 Skills Demonstrated

Financial performance analysis · P&L modeling · KPI design & interpretation · cost structure & profitability analysis · DAX calculation groups & time intelligence · Power BI report development · business-focused data storytelling
