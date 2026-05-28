# Bank Loan Analysis — Lending KPIs & Portfolio Quality Dashboard 📊

An interactive 3-view Tableau dashboard analyzing a **38.6K-loan, $435.7M-funded** lending portfolio — built to give lenders, asset managers, and finance teams self-service visibility into portfolio quality, repayment behavior, and borrower segments.

![Tableau](https://img.shields.io/badge/Tableau-E97627?style=flat&logo=tableau&logoColor=white)
![Data%20Modeling](https://img.shields.io/badge/Data%20Modeling-1f6feb?style=flat)
![BI%20Dashboard](https://img.shields.io/badge/BI%20Dashboard-2ea44f?style=flat)

---

## 📌 Overview

This project presents an interactive lending-portfolio dashboard built in Tableau on a real-world loan dataset. It packages the questions a lending or asset-management team asks every reporting cycle — *How is the portfolio performing? What's the good-vs-bad loan split? Where is risk concentrated? Which loans drove the trend?* — into three connected views with shared filters and drill-down to loan-level detail.

---

## 🎯 Business Problem

Lenders need recurring visibility into:

- **Portfolio performance** — month-over-month (MoM) and month-to-date (MTD) movement on funded amount, received payments, interest rate, and DTI
- **Portfolio quality** — what share of loans is **good** (Fully Paid / Current) versus **bad** (Charged Off), and the dollar exposure of each
- **Segment drivers** — which loan purposes, grades, terms, states, employment lengths, and home-ownership profiles drive risk or revenue
- **Loan-level traceability** — the ability to drill from headline KPIs down to individual loan records without going back to the analyst

This dashboard answers all of the above in a single self-service experience.

---

## 📊 Dashboard Preview

**Summary**
![Summary view](images/Dashboard%201.png)

**Overview**
![Overview view](images/Dashboard%202.png)

**Details**
![Details view](images/Dashboard%203.png)

---

## 📂 Dataset

- **File:** `financial_loan.csv`
- **Records:** 38,574 loan applications
- **Domain:** Consumer lending (LendingClub-style schema)

| Field | Type | Used For |
|---|---|---|
| `id` | Identifier | Loan-level drill-down |
| `loan_amount`, `funded_amount`, `total_payment` | Numeric | KPI aggregates |
| `int_rate`, `installment`, `dti` | Numeric | Risk & cost metrics |
| `grade`, `sub_grade` | Categorical | Credit segmentation |
| `purpose` | Categorical | Loan-purpose breakdown |
| `home_ownership` | Categorical | Borrower profile |
| `emp_length` | Categorical | Employment-tenure analysis |
| `verification_status` | Categorical | Global filter |
| `loan_status` | Categorical | Good-vs-bad classification |
| `issue_date` | Date | Time series, MTD, MoM |
| `address_state` | Categorical | Geographic analysis |

**"Good" loans** = `Fully Paid` + `Current`. **"Bad" loans** = `Charged Off`.

---

## 🧩 Dashboard Structure

### 1. Summary
Executive snapshot. Headline KPIs with **MTD** and **MoM** deltas, portfolio-quality donuts (Good vs. Bad), and a loan-status breakdown table.

### 2. Overview
Six categorical breakdowns wired to a **parameter-driven measure selector**, so the same view can be re-cast across loan amount, funded amount, or total received:

- Trend by month (time series)
- Geographic distribution by state (US map)
- Term split — 36 months vs. 60 months (donut)
- Employment length (bar)
- Loan purpose (bar)
- Home ownership — mortgage / rent / own (segment)

### 3. Details
Loan-level table with `id`, purpose, home ownership, grade, sub-grade, issue date, loan amount, interest rate, installment, and total payment — supporting ad-hoc lookup of any record an aggregate view raises questions about.

All three views share global slicers: **Purpose**, **Grade**, **Verification Status**.

---

## 📐 Key Metrics & KPIs

| KPI | Definition | Snapshot |
|---|---|---|
| Total Loan Applications | Count of loan IDs | 38.6K |
| Total Funded Amount | Sum of disbursed principal | $435.7M |
| Total Amount Received | Sum of repayments received | $473.0M |
| Average Interest Rate | Mean of `int_rate` | 12.0% |
| Average DTI | Mean of `debt_to_income` ratio | 13.3% |
| Good Loan % (by applications) | (Fully Paid + Current) / Total | 86.2% |
| Bad Loan % (by applications) | Charged Off / Total | 13.8% |

Each KPI is also tracked with a **rolling MTD** and **MoM delta** to flag acceleration or deceleration in real time.

---

## 🔍 Observations from the Data

A few things this dashboard makes obvious at a glance:

- **Portfolio quality looks healthy at the headcount level (86.2% good)**, but the bad-loan slice still represents **$65.5M of funded principal** with only **$37.3M recovered** — a material loss exposure worth segmenting by grade and verification status.
- **Debt consolidation dominates loan purpose**, driving ~$254M of received payments — roughly 4× the next-largest category (credit card at ~$65M).
- **Borrowers with 10+ years of employment** account for the largest share of repayments (~$126M), suggesting employment tenure correlates with reliable repayment.
- **36-month terms outweigh 60-month terms (~62% / 38%)**, consistent with the safer, smaller-balance profile of debt-consolidation lending.
- **Mortgage-holding borrowers outperform renters** in total repayments received, which has implications for any model predicting default risk.

---

## 🛠 Tools & Tech

- **Tableau Desktop** — workbook authoring, calculated fields, parameters, dashboard actions
- **Data modeling** — relationships between loan, borrower, and payment dimensions
- **Calculated fields** — Good/Bad classification, MTD, MoM, segment shares
- **Parameters** — measure selector to recast charts dynamically

---

## ▶️ How to Open

1. Install [Tableau Desktop](https://www.tableau.com/products/desktop) or the free [Tableau Reader](https://www.tableau.com/products/reader).
2. Clone the repo:
```bash
   git clone https://github.com/Pavanijain03/bank_loan_analysis.git
   cd bank_loan_analysis
```
3. Open `Finance domain.twb` in Tableau. Make sure `financial_loan.csv` is in the same directory — the workbook references it by relative path.
4. Use the **Summary / Overview / Details** buttons in the left rail to navigate. Use the global slicers (Purpose, Grade, Verification Status) to filter all three views.

---

## 📁 Repository Structure
bank_loan_analysis/
├── Finance domain.twb       # Tableau workbook (3-view dashboard)
├── financial_loan.csv       # Source dataset (38,574 loans)
├── images/
│   ├── Dashboard 1.png      # Summary view
│   ├── Dashboard 2.png      # Overview view
│   └── Dashboard 3.png      # Details view
└── README.md

---

## 💡 What I Learned

- **Designing for the reader, not the data.** The Summary → Overview → Details progression mirrors how a finance team actually consumes information: headline first, segmentation second, transaction-level only when something looks off.
- **Parameter-driven measure selection** keeps the chart count down without sacrificing analytical depth — one chart, many questions.
- **MTD and MoM are deceptively hard.** Getting these right required careful handling of the issue-date calendar, partial-month boundaries, and reference-date logic — a useful primer for any cohort or trending analysis.
- **Self-service vs. governance is a real tradeoff.** Opening filters to stakeholders means thinking carefully about which dimensions can safely combine and which would produce misleading aggregates.

---

## 👤 Author

**Pavani Jain** — MS in Artificial Intelligence, Northeastern University

[LinkedIn](https://www.linkedin.com/in/pavani-jain-ab6488204/) · [GitHub](https://github.com/Pavanijain03) · [Portfolio](https://pavanijain03.github.io/) · jain.pav@northeastern.edu

> If you find this project useful or want to talk about lending analytics, BI dashboarding, or making the jump from Tableau to Power BI, I'd love to connect.
