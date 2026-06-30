# NovaTech Solutions — Workforce Attrition Analysis

**Role:** Data Analyst — independent portfolio project (data cleaning, modelling, dashboard design, and insight)
**Tools:** Python (data cleaning) · Power BI (modelling & dashboard) · DAX
**Scope:** 1,200 employees · FY2024 · UK technology firm *(fictitious dataset built for portfolio analysis)*

---

## The business question

Every employee who leaves costs money to replace and takes institutional knowledge with them. NovaTech's leadership knew attrition was a problem but not *where* it was concentrated, *who* it was affecting, or *what it was costing*.

I built a four-page Power BI dashboard to answer three questions a People or Finance leader would actually ask: **How bad is it, what is it costing us, and where should we intervene first?**

---

## Approach

I started with a raw HR dataset and cleaned it in Python (handling missing values, standardising categories, deriving tenure bands), then exported a clean file into Power BI. There I built a single, consistent attrition-rate measure in DAX and layered the analysis across four views, an overview, a financial-impact page, a drivers page, and a diversity-and-equity page, each one led by a headline finding rather than just charts.

A deliberate design choice ran through the whole build: **every page states its own key finding and recommendation in plain business language.** The dashboard is meant to be *read*, not just *looked at*.

---

## Methodology & data preparation

The raw dataset was cleaned and feature-engineered in Python before modelling in Power BI. Key decisions, all logged in the [cleaning notebook](notebooks/NovaTech_Analysis_Workforce_Attrition.ipynb):

- **Data quality:** removed 24 duplicate employee records (whitespace in IDs was masking one of them until stripped), nulled 5 future-dated hire entries and 8 out of range performance scores, and reconciled logical mismatches between `left_company` and `exit_date` — flagging 15 leavers with missing exit dates rather than dropping them, to preserve attrition accuracy.
- **Missing values:** numeric fields imputed with department-level medians; `gender` and `ethnicity` nulls set to "Not disclosed" rather than guessed.
- **Feature engineering:** derived tenure bands, hire year, and a composite **flight-risk score** (engagement weighted 60%, performance 40%; top quartile flagged high-risk).
- **Cost estimation — an explicit assumption:** the dataset contains no salary data, so replacement cost is a **proxy** — UK sector midpoints by job level (CIPD benchmarks) × the industry-standard 60% replacement rate. The £7.79M figure is therefore an informed estimate, not actuals, and should be read as directional.
- **Validation:** the flight-risk flag was tested in both Python and DAX, flagged employees left at **36.4%** vs the 19.1% baseline (1.9× lift), with both methods agreeing.

---

## What I found

**1. Attrition is a front-office problem, not a company-wide one.**
The company-wide attrition rate was **19.1%**, but that average hides the real story. Sales & Commercial (**33%**) and Marketing (**29%**) drove the majority of exits, while functions like People & Culture and Finance sat far lower. Treating attrition as one number would have led to a blanket fix; the data pointed to a targeted one.

**2. It's expensive and the cost is concentrated.**
Total replacement cost for the year reached **£7.79M**, with Sales & Commercial alone accounting for **£2.60M (33%)**. The top three departments represented **68%** of all replacement cost — meaning retention investment aimed at a handful of teams would recover most of the value.

**3. Early-tenure and junior staff leave fastest.**
Employees in their earliest tenure band showed the highest exit rate *(confirm exact % from the Tenure Band chart before publishing)*, and analyst-level attrition was the highest of any job level. This reframes the problem from "retain everyone" to "fix the first year, especially in customer-facing roles."

**4. Engagement predicts exits — and the flag is usefully sharp.**
Leavers scored markedly lower on engagement (**3.0** vs **3.5** for stayers). A flight-risk flag I built captured this: employees flagged high-risk left at a **36.4% rate — nearly double the 19.1% company average**, making it a practical early-warning signal for managers.

**5. Disparities only appear at the intersection.**
On the surface, attrition looked broadly even across ethnicity (a **7.0-point** gap between the highest and lowest groups). But crossing ethnicity with job level revealed sharper, localised disparities that the headline numbers concealed — and the company-wide gender gap (men leaving 4.0 points more than women) actually *reverses* inside some departments. In Sales & Commercial, men and non-binary staff left at higher rates than women — a pattern invisible at the aggregate level.

---

## Handling sensitive data responsibly

The diversity page raised a real analytical risk: splitting 1,200 people across ethnicity, gender, and job level creates small groups, where a single departure can look like a dramatic trend.

To avoid drawing conclusions from noise and to avoid identifying individuals **I suppressed any cell with fewer than 10 people**, showing it as blank rather than a misleading rate. For example, "Not disclosed" gender (24 people firm-wide) appears in aggregate views but is correctly suppressed at department level, where no group reaches the threshold.

Knowing *when not to report a number* is part of doing this work credibly.

---

## Recommendations

1. **Target retention spend** at Sales & Commercial and Marketing rather than across the board.
2. **Redesign the first-year experience** for early-tenure and analyst-level staff in customer-facing functions.
3. **Use the flight-risk flag** to give managers an early-warning list, paired with quarterly engagement check-ins in the highest-gap departments.

---

## What this project demonstrates

End-to-end ownership: cleaning messy data, modelling it, designing a decision-ready dashboard, and most importantly translating numbers into the business decisions they support. It also reflects a careful approach to sensitive demographic data, which matters in any people-analytics context.

