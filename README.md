# Digital-Marketing-Performance-Analysis (Jan - Dec 2023)

<img width="836" height="455" alt="Screenshot 2026-03-28 001153" src="https://github.com/user-attachments/assets/d639ce25-acec-4b1b-ab40-ddf2a9f0a4c8" />

## Table Of Contents
[Business Problem Definiton](#business-problem-definition)

[Dashboard Wireframing](#dashboard-wireframing)

[Data Import And Cleaning](#data-import-and-cleaning)

[Feature Engineering And Data Modelling](#feature-engineering-and-data-modelling)

[KPIs](#kpis)

[Answering Business Questions](#answering-business-questions)

[Dashboard Development](#dashboard-development)

[Executive Summary And Insights](#executive-summary-and-insights)

[Strategic Recommendations for Performance Optimization](#strategic-recommendations-for-performance-optimization)

----------------------------------------------------------------------------------------------------------------------------------------------
## Business Problem Definition
### Before touching the data, the following business questions were defined:
How much was spent on digital campaigns in 2023 and what value did they generate?

Which campaign group (Spring, Summer, Fall) delivered the highest ROAS?

Does higher spend translate into higher conversions and conversion value?

How does performance vary by channel, device, and city?

Are there month-on-month (MoM) performance changes that indicate seasonality?

Which optimisation levers (channel, device, geography) can improve efficiency?

These questions guided metric selection, modelling, and visual design.

------------------------------------------------------------------------------------------------------------------------------------------------
## Dashboard Wireframing
Before analysis, a low-fidelity wireframe was created in PowerPoint to define:
Page 1 – Performance Overview
KPI cards (Spend, Conversions, Conversion Value, ROAS, CPA)


Spend vs Conversion Value by Campaign Group


ROAS by Campaign Group


Monthly Trend in Conversions


Page 2 – Optimisation & Drivers
Channel Effectiveness by Campaign Group


Device Performance Impact


Geographic Performance (Conversion Value share)


City-level ROAS comparison


This step ensured the analysis stayed focused and business-driven.

------------------------------------------------------------------------------------------------------------------------------------------------
## Data Import And Cleaning 
Import Process
Dataset imported into Excel Power Query


Data source contained campaign, date, channel, device, city, spend, conversions, and engagement metrics


Data Type Standardisation
Spend, CPC, Conversion Value → Decimal number


Conversions, Impressions, Clicks → Whole number


Campaign Group, Channel, Device, City → Text


### Date Issue Resolution
The dataset contained mixed date formats:
03/01/2023


3/14/2023


To resolve this:
The Date column was explicitly selected


Correct casing of column name was used


Date type was enforced in Power Query using Locale-aware parsing


Final format:
 dd-MM-yyyy

------------------------------------------------------------------------------------------------------------------------------------------------
## Feature Engineering And Data Modelling

### Calendar Table Creation (Power Pivot)
A dedicated Calendar (Date) table was created to enable time intelligence.
Calendar Table Fields
Date


Year


Month Name


Month Number


Year-Month


### Relationship Modeling
Calendar[Date] → FactTable[Date]


One-to-many relationship


Calendar table marked as Date Table


This enabled MoM calculations and proper sorting by month number.

-----------------------------------------------------------------------------------------------------------------------------------------------
## KPIS
### (DAX)

Total Spend
```Total Spend = SUM(FactTable[Spend])```

 This is the total amount invested in marketing campaigns, forming the baseling against which performance is evaluated.

Total Conversions
```Total Conversions = SUM(FactTable[Conversions])```
The number of completed actions, indicating how much customer response the campaigns generated.


Conversion Value
```Total Conversion Value = SUM(FactTable[Conversion Value])```
The total revenue from conversions, showing the actual business impact of campaign activity.

ROAS (Return on Ad Spend)
```ROAS = DIVIDE([Total Conversion Value], [Total Spend])```
Measures how much revenue is earned per unit of spend, revealing whether campaigns are profitable.

CPA (Cost Per Acquisition)
```CPA = DIVIDE([Total Spend], [Total Conversions])```
Show the average cost to acquire a customer, indicating how expensive it is to drive conversions.

While conversions shows volume. ROAS and CPA reveal whether that volume is being achieved efficiently and profitably.

Infinity values occurred where conversions = 0


These were handled logically and excluded from averages



### Month-on-Month (MoM) Calculations
Previous Month Spend
``PM Spend =
CALCULATE(
    [Total Spend],
    FILTER(
        ALL('Calendar'),
        'Calendar'[Date]
            = EOMONTH(MAX('Calendar'[Date]), -1)
    )
)``


MoM Spend Growth %
``MoM Spend Growth % =
DIVIDE(
    [Total Spend] - [PM Spend],
    [PM Spend]
)``


Same logic applies to other KPIs
Previous Month Conversions
``PM Conversions =
CALCULATE(
    [Total Conversions],
    FILTER(
        ALL('Calendar'),
        'Calendar'[Date]
            = EOMONTH(MAX('Calendar'[Date]), -1)
    )
)``

Previous Month ROAS
``PM ROAS = CALCULATE( [ROAS], FILTER( ALL('Calendar'), 'Calendar'[Date] = EOMONTH(MAX('Calendar'[Date]), -1) ) )``

Same logic applied to:
Conversions


Conversion Value


ROAS


CPA

-----------------------------------------------------------------------------------------------------------------------------------------------

## Answering Business Questions
Pivot tables were used to answer each question:
Campaign Group × ROAS


Channel × Campaign Group


Device × Campaign Group


City × ROAS


Monthly trends (using Calendar table)


Conditional formatting was applied to highlight performance differences, not raw volume.

-----------------------------------------------------------------------------------------------------------------------------------------------

## Dashboard Development 



<img width="836" height="455" alt="Screenshot 2026-03-28 001153" src="https://github.com/user-attachments/assets/d639ce25-acec-4b1b-ab40-ddf2a9f0a4c8" />



<img width="1139" height="510" alt="Screenshot 2026-03-28 001217" src="https://github.com/user-attachments/assets/f02c7ed1-ffc8-456e-9c69-8f3ca8c86833" />

-----------------------------------------------------------------------------------------------------------------------------------------------
## Executive Summary And Insights

Report Period: January – December 2023


Overall, the digital marketing accounts maintained high health and efficiency throughout 2023. With a total spend of £163,181.00, the campaigns generated £1,731,700.26 in conversion value, resulting in an annual ROAS of £10.61. Most importantly, the account demonstrated "efficient scaling"—a 6.78% increase in spend was met with a 7.85% increase in conversions, indicating that the target audience is not yet saturated.
### Key Performance Insights

### 1. Seasonal Efficiency Disparity

While the Fall campaign group drove the highest volume (representing roughly 48% of total spend), it was the least efficient segment.

  Top Performer: Summer yielded the highest ROAS at £14.10.

  Volume Driver: Fall yielded a ROAS of £9.45.

  Insight: The current strategy over-indexes on high-volume, lower-efficiency periods. There is significant "headroom" to scale the Summer and Spring segments where the return on investment is substantially higher.

### 2. Regional Performance (The "Birmingham" Opportunity)

Geographic data reveals a stark contrast in efficiency across the UK.

  Efficiency Leader: Birmingham is the standout region with a ROAS of £13.94.

  The London Gap: While London accounts for 31% of total conversion value, its efficiency (£7.98 ROAS) is nearly half that of Birmingham.

  Insight: We are paying a premium for conversions in London. Reallocating a portion of the London budget to Birmingham and Manchester could lower the aggregate CPA.

### 3. Channel & Device Drivers

  Channel Effectiveness: Pinterest is the most consistent driver of effectiveness across all campaign groups (Spring, Summer, and Fall). Facebook remains the weakest direct-conversion channel.

  Device Preference: Desktop consistently outperforms Mobile. This suggests that while customers may browse on mobile, the final high-value conversion is happening on larger screens.
  
-----------------------------------------------------------------------------------------------------------------------------------------------
## Strategic Recommendations for Performance Optimization

### 1. Seasonal Budget Rebalancing

The current allocation is heavily weighted toward the "Fall" campaign group, which yields the lowest ROAS (£9.45). Conversely, the "Summer" group is significantly underfunded despite being the most efficient (£14.10 ROAS).

Action: Reallocate 15% of the Fall budget to Summer and Spring evergreen segments.

Objective: Maximize the higher marginal return found in these efficient periods to increase the total annual conversion value without increasing the top-line budget.

### 2. Regional Bid Refinement (The Birmingham Pivot)

Data shows a massive efficiency gap between major cities. Birmingham delivers a ROAS of £13.94, while London—despite contributing 31% of the total value—operates at a much lower £7.98 ROAS.

Action: Implement a +20% bid adjustment for Birmingham and Manchester to capture more of this high-intent traffic. Concurrently, apply a -20% bid adjustment to London to reduce the average cost per acquisition (CPA).

Objective: Normalize efficiency across regions and reduce "wasted" spend in the more expensive London market.

### 3. Pinterest-Driven Creative Strategy

Pinterest is the standout performer for "Channel Effectiveness" across all seasons. This suggests the visual style or messaging on Pinterest is resonating most with the target audience.

Action: Conduct a creative audit of the top-performing Pinterest assets. Adapt these visual elements (e.g., specific layouts, color palettes, or lifestyle imagery) for use in Facebook and Instagram campaigns.

Objective: Bridge the effectiveness gap on Meta platforms by utilizing proven creative winners from Pinterest.

### 4. Technical UX Audit for Mobile Conversion

There is a consistent performance gap between Desktop and Mobile users. While Mobile often serves as a discovery tool, the significantly higher ROAS on Desktop suggests friction in the mobile purchase journey.

Action: Launch a technical audit focused on Mobile Site Speed and the Checkout Flow. Ensure that "One-Click" payment options (like Apple Pay or Google Pay) are prominent to reduce mobile abandonment.

Objective: Close the efficiency gap between devices and improve the overall mobile conversion rate.

### 5. Post-Peak Retention & December Recovery

The sharp decline in conversions in December (1,321) compared to the October/November peak (~4,600) suggests a major seasonal drop-off or audience exhaustion.

Action: Introduce a Retargeting-only campaign in early December specifically targeting "Warm Leads"—users who clicked or added to cart during the Fall peak but did not convert.

Objective: Capture residual value from the high-traffic autumn months and mitigate the year-end slump.
