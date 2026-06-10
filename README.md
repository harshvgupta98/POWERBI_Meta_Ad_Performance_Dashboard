# Meta Ad Performance Dashboard

A Power BI dashboard analysing Meta advertising performance across Facebook and Instagram — covering 400,000 ad events, 200 ads, 50 campaigns, and 9,841 users.

---

## Short Description

A Power BI dashboard built to analyse Meta ad performance across Facebook and Instagram. It tracks key metrics like impressions, clicks, CTR, engagement rate, and purchase rate — broken down by platform, ad type, age, gender, country, and time. Built using a star schema data model with DAX measures and a dynamic measure slicer for flexible KPI analysis.

---

## Business Objective

The goal of this dashboard is to give marketing teams a clear view of how their Facebook and Instagram ad campaigns are performing. It helps answer questions like which platform drives better results, which ad formats convert best, which audiences engage most, and when to schedule ads for maximum impact.

---

## Tools Used

- Power BI Desktop
- Power Query
- DAX
- Data Modelling (Star Schema)

---

## Data Source

4 CSV files covering ad interactions, ad details, campaign budgets, and user demographics.

| Table | Rows | Description |
|-------|------|-------------|
| ad_events | 400,000 | Every ad interaction — impressions, clicks, likes, comments, shares, purchases |
| ads | 200 | Ad details — platform, type, target gender, age group, interests |
| campaigns | 50 | Campaign name, budget, start/end dates, duration |
| users | 9,841 | User demographics — gender, age group, country, interests |

Date Range: May 2025 – August 2025 | Platforms: Facebook & Instagram | Countries: 10

---

## Data Model

Star schema with `ad_events` as the fact table connected to `ads`, `campaigns`, `users`, and a `Calendar Table`. A disconnected `Select Dynamic Measure` table drives the dynamic slicer.

- `campaigns` → `ads` → `ad_events` (one-to-many chain)
- `users` → `ad_events` (one-to-many)
- `Calendar Table` → `ad_events` (one-to-many via Event Date)
- `Event Date` and `Event Hour` calculated columns extracted from timestamp in Power Query

---

## DAX Measures

| Measure | Expression | Description |
|---------|------------|-------------|
| Impressions | `CALCULATE(COUNTROWS(ad_events), ad_events[event_type] = "Impression")` | Total times ads were displayed |
| Clicks | `CALCULATE(COUNTROWS(ad_events), ad_events[event_type] = "Click")` | Total clicks on ads |
| Engagements | `CALCULATE(COUNTROWS(ad_events), ad_events[event_type] IN {"Click","Share","Comment"})` | Clicks + Shares + Comments |
| Purchases | `CALCULATE(COUNTROWS(ad_events), ad_events[event_type] = "Purchase")` | Total purchases from ads |
| CTR | `DIVIDE([Clicks], [Impressions])` | % of impressions that resulted in clicks |
| Engagement Rate | `DIVIDE([Engagements], [Impressions])` | % of impressions that resulted in engagements |
| Conversion Rate | `DIVIDE([Purchases], [Clicks])` | % of clicks that resulted in purchases |
| Purchase Rate | `DIVIDE([Purchases], [Impressions])` | % of impressions that resulted in purchases |
| Total Budget | `SUM(campaigns[total_budget])` | Total budget across all campaigns |
| Avg Budget / Campaign | `AVERAGE(campaigns[total_budget])` | Average budget per campaign |
| Dynamic Measure | `SWITCH(SELECTEDVALUE('Select Dynamic Measure'[Select Dynamic Measure]), "Impressions", [Impressions], "Clicks", [Clicks], "Engagements", [Engagements], "Purchases", [Purchases], "CTR", [CTR], "Engagement Rate", [Engagement Rate], "Conversion Rate", [Conversion Rate], "Purchase Rate", [Purchase Rate])` | Returns selected KPI for dynamic chart rendering |

---

## What I Built

- Platform switcher — Facebook vs Instagram in one click
- 12 KPI cards across two rows (volume and rate metrics)
- Custom tooltip page showing all 12 KPIs on hover
- Engagements by Gender (donut chart)
- Engagements by Age (bar chart)
- Weekly Engagements Trend — stacked by ad type
- Hourly Engagements Trend — area chart by hour of day
- Engagements by Country — bubble map
- Analysis by Month — calendar heat map
- Analysis by Ad Type — matrix table with CTR, PR, ER, CR
- Dynamic Measure slicer — one chart switchable across any KPI
- Campaign Name and Target Interests slicers

---

## Key Numbers

| Metric | Overall | Facebook | Instagram |
|--------|---------|----------|-----------|
| Impressions | 339.8K | 216.0K | 123.8K |
| Clicks | 40.08K | 25.39K | 14.69K |
| Purchases | 2.0K | 1.3K | 708 |
| CTR | 11.79% | 11.76% | 11.86% |
| Engagement Rate | 13.58% | 13.56% | 13.60% |
| Conversion Rate | 5.07% | 5.21% | 4.82% |
| Purchase Rate | 0.60% | 0.61% | 0.57% |
| Total Budget | $3M | $3M | $3M |

---

## Key Findings

- CTR of 11.79% is well above the industry average of 1–2%
- High CTR and engagement but low Purchase Rate (0.60%) — strong top-funnel, weak bottom-funnel
- Video ads have the highest CTR (11.88%) and Engagement Rate (13.74%)
- Stories ads have the highest Purchase Rate (0.65%) — best format for conversions
- Females aged 18–30 show the highest engagement across both platforms
- Engagement peaks between 15:00–20:00 hours daily
- US and India lead in engagement volume; Germany and UK show stronger conversion potential

---

## How to Use

1. Download all CSV files and `Meta___Facebook_Analysis.pbix`
2. Open the `.pbix` file in Power BI Desktop
3. If prompted, reconnect to the CSV files as data sources
4. Use the Facebook / Instagram buttons to switch platform views
5. Use the Dynamic Measure dropdown to change the metric across charts
6. Hover over any visual to trigger the custom KPI tooltip

> The `.pbit` template file does not contain data — connect to the CSV files on first open.

---

## Dashboard Preview

**Facebook View**

![Meta Ad Performance Dashboard — Facebook](Snapshot_Meta_Facebook.png)

**Instagram View**

![Meta Ad Performance Dashboard — Instagram](Snapshot_Meta_Instagram.png)

**Custom Tooltip**

![Custom KPI Tooltip](Snapshot_ToolTip.png)
