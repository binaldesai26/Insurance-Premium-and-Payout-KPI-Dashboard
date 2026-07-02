# Insurance Premium & Payout KPI Dashboard | Power BI

An interactive Power BI dashboard analyzing life insurance policy performance — premiums, payouts, claims, loan exposure, and regional sales performance — built on a star-schema data model with a Fact table and six Dimension tables.

## Project Overview

Insurance companies sit on large volumes of policy, premium, and claims data spread across multiple business functions — sales agents, regional/zonal management, policy types, and customer demographics. This project consolidates that data into a single Power BI model to answer questions insurance business leaders actually ask:

- Which regions and zones are driving premium growth, and which are lagging?
- How exposed is the business to loan-eligible policies vs. total sum assured?
- What does the claims-to-active-policy ratio look like across policy types?
- Which sales agents and policy protection plans are performing best?
- How is premium collection trending year-over-year?

## Data Model

The dashboard is built on a **star schema** with 1 Fact table and 6 Dimension tables, joined on their respective keys:

| Table | Type | Key Fields |
|---|---|---|
| `FCT_Insurance_Policy_Table` | Fact | Policy Number, Premium Amount, Sum Assured, Loan Amount, Claim ID, Purchase Year/Quarter/Month |
| `DM_Customer_Detail_Table` | Dimension | Customer ID, Age, Occupation, Smoker Status, State |
| `DM_Insurance_Agent_Table` | Dimension | Agent Code, Sales Agent, State |
| `DM_Policy_Type` | Dimension | Policy Type Code, Policy Type (Endowment / Whole / Universal) |
| `DM_Policy_Protection_Plan` | Dimension | Policy Code, Policy Name, Business Code |
| `DM_Regional_Manager` | Dimension | Regional Manager ID, Region, States Covered |
| `DM_Zonal_Manager` | Dimension | Zonal Manager ID, Zonal Manager Name |

The model also includes a `Hierarchy Table` and `Region` table (used to build the Zonal Manager → Regional Manager → Agent drill-down on the Sales Hierarchy page) plus Power BI's auto date tables for time intelligence.

**Grain of the fact table:** one row per insurance policy, with attributes covering the full policy lifecycle — start date, last paid date, tenure, purchase date, premium amount, payment frequency, loan eligibility, underwriting expenses, claim ID, and current policy status (Active/Lapsed/Claimed etc.).

## Key Metrics & DAX Measures

12 custom DAX measures built on top of the fact table:

| Measure | Logic |
|---|---|
| `Total Premium_Amount` | `SUM('FCT Insurance_Policy_Table'[Total Premium Amount])` |
| `Total Annual_Premium` | `SUM('FCT Insurance_Policy_Table'[Total Annual Premium])` |
| `Total Premium_Paid` | `SUM('FCT Insurance_Policy_Table'[Total Premium Paid])` |
| `Total Premium_Payable` | `SUM('FCT Insurance_Policy_Table'[Total Premium Payable])` |
| `% Premium Paid` | `DIVIDE([Total Premium_Paid], [Total Premium_Amount], 0)` |
| `% Premium Payable` | `DIVIDE([Total Premium_Payable], [Total Premium_Amount], 0)` |
| `Profit/Gain` | `SUM(Maturity Amount) - SUM(Total Premium Amount)` |
| `Annual Amount Growth` | `DIVIDE(SUM(Profit / Gain), SUM(Tenure (Years)), 0)` |
| `Annual ROI` | `DIVIDE([Annual Amount Growth], SUM(Total Annual Premium))` |
| `Annual Growth Rate Fixed` | `DIVIDE([Annual Amount Growth], SUM(Total Annual Premium), 0)` |
| `CAGR (%)` | Compound growth from Total Premium Paid → Maturity Amount over policy Tenure, using `POWER(DIVIDE(FinalValue, InitialValue), 1/Years) - 1` |
| `Underwriting_expenses` | `SUM('FCT Insurance_Policy_Table'[Underwriting expenses])` |

**Design note worth calling out:** the model deliberately separates `Annual ROI` (simple return: gain ÷ premium) from `CAGR (%)` (compounded annual return using the `POWER()` function over policy tenure) — these answer different business questions and are easy to conflate. This distinction is worth explaining clearly in an interview or a teaching video.

## Dashboard Pages

The report has 6 pages:
1. **Summary** — top-level KPIs and overview
2. **Insurance Overview** — policy and premium breakdown
3. **Investment Value vs Maturity Value** — comparing what was paid in vs. what matures out
4. **Annual Premium vs Protection Value** — premium paid against sum assured/coverage
5. **Premium Analysis (5–20 Yrs)** — premium trends across policy tenure bands
6. **Sales Hierarchy** — performance by Zonal Manager → Regional Manager → Agent

## Screenshots

*(add exported PNGs to the `screenshots/` folder and reference them here, e.g.)*
```
![Executive Summary](screenshots/executive-summary.png)
![Regional Performance](screenshots/regional-performance.png)
```

## Tools Used

- **Power BI Desktop** — data modeling, DAX, visualization
- **Power Query** — data cleaning and transformation
- **DAX** — calculated measures and KPIs
- **Star Schema Data Modeling**

## How to Use

1. Download `Insurance_Premium_and_Payout_KPI_Dashboard.pbix`
2. Open in Power BI Desktop (free download from Microsoft)
3. Explore the report pages; slicers allow filtering by Region, Policy Type, and Year

## About Me

I'm Binal Desai, a Data Analytics professional from Vadodara, India with 6+ years of experience in MIS, reporting, and dashboard development across the healthcare and insurance sectors. This project reflects my transition from advanced Excel-based reporting into Power BI, applying my domain knowledge of insurance operations to a full end-to-end dashboard build — from data modeling to DAX to business storytelling.

- LinkedIn: [linkedin.com/in/binalkdesai](https://www.linkedin.com/in/binalkdesai/)
- GitHub: [github.com/binaldesai26](https://github.com/binaldesai26)

## Data Note

This dataset is a simulated/practice insurance dataset used for learning and portfolio purposes; it does not represent real customer or company data.
