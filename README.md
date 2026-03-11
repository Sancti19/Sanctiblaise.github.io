# Top UK YouTubers 2024 — Data Analysis Project

## 📋 Table of Contents
- [Objective](#objective)
- [Data Source](#data-source)
- [Stages](#stages)
- [Design](#design)
  - [Dashboard Components](#dashboard-components)
  - [Dashboard Mockup](#dashboard-mockup)
  - [Tools Used](#tools-used)
- [Development](#development)
  - [Pseudocode](#pseudocode)
  - [Data Exploration](#data-exploration)
  - [Data Cleaning](#data-cleaning)
  - [Data Transformation & SQL View](#data-transformation--sql-view)
- [Testing](#testing)
  - [Data Quality Checks](#data-quality-checks)
- [Visualization](#visualization)
  - [Dashboard Results](#dashboard-results)
  - [DAX Measures](#dax-measures)
- [Analysis](#analysis)
  - [Findings](#findings)
  - [Validation](#validation)
  - [Discovery](#discovery)
- [Recommendations](#recommendations)
  - [Potential ROI](#potential-roi)
  - [Action Plan](#action-plan)
- [Conclusion](#conclusion)

---

## 🎯 Objective

The Head of Marketing needs to identify the **top UK-based YouTube creators** most suitable for brand collaboration and sales promotion campaigns.

Brands increasingly rely on influencer marketing to reach targeted audiences. However, choosing the right influencers requires a data-driven approach rather than relying solely on popularity.

### This analysis aims to:
- Identify top-performing UK YouTubers
- Evaluate reach, engagement potential, and content activity
- Determine which creators provide the highest marketing value
- Support the brand in selecting influencers who can maximize campaign effectiveness and ROI

### User Story
> *As the Head of Marketing, I want to identify top-performing UK YouTubers who can effectively promote our products so that we can increase brand awareness, reach the right audience, and drive sales through influencer partnerships.*

The analysis should allow the marketing team to:
- Quickly identify YouTubers with the largest audience reach based on subscriber counts
- Understand which creators generate high engagement through video views
- Compare creators based on average views per video, not just subscriber numbers
- Identify consistent content creators who regularly publish videos
- Evaluate which channels provide the best marketing value for collaboration

---

## 📊 Data Source

**What data do we need?**

A UK YouTuber dataset containing the following key variables:

| Field | Description |
|---|---|
| Channel Name | Name of the YouTube channel |
| Subscribers | Total subscriber count |
| Total Views | Cumulative views across all videos |
| Total Videos | Total number of videos uploaded |

> 📥 The data is sourced from **Kaggle** (Excel extract).

---

## 🔄 Stages

1. Design
2. Development
3. Testing
4. Analysis

---

## 🎨 Design

### Dashboard Components

Before building the dashboard, a conceptual layout was planned to determine which metrics would be displayed. The dashboard was designed to answer:

1. Who are the top 10 YouTubers with the most subscribers?
2. Which 3 channels have uploaded the most videos?
3. Which 3 channels have the most views?
4. Which 3 channels have the highest average views per video?
5. Which 3 channels have the highest views per subscriber ratio?
6. Which 3 channels have the highest subscriber engagement rate per video uploaded?

### Dashboard Mockup
![Dashboard Mockup](https://github.com/Sancti19/Sanctiblaise.github.io/blob/main/assets%20/images/dashboard_mockup.png
)
Data visuals used in the dashboard include:

- 📋 Table
- 🌳 Treemap
- 🃏 Scorecards
- 📊 Horizontal bar chart

### Tools Used

| Tool | Purpose |
|---|---|
| Excel | Initial data exploration and formatting |
| SQL | Data cleaning, transformation, testing and query analysis |
| Power BI | Data visualization and interactive dashboard creation |
| GitHub | Hosting the project documentation and version control |
| Mokkup AI | Designing mockup of dashboard |

---

## 🛠️ Development

### Pseudocode

The analysis followed these steps:

1. Get the data
2. Explore the data in Excel
3. Load the data into SQL Server
4. Clean the data with SQL
5. Test the data with SQL
6. Calculate analytical metrics
7. Create SQL views for reporting
8. Import SQL views into Power BI
9. Visualize the data in Power BI
10. Generate findings based on the insights
11. Write the documentation and commentary
12. Publish the data to GitHub Pages

### Data Exploration

Initial observations from the raw dataset:

1. At least **4 columns** contain the data needed for analysis — no need to request additional data from the client
2. The **first column contains channel IDs** separated by a `@` symbol — channel names need to be extracted
3. Some **cells and header names are in a different language** — these needed to be reviewed and addressed
4. The dataset contains **more columns than needed** — irrelevant columns were removed

### Data Cleaning

**Expected clean data criteria:**

| Property | Description |
|---|---|
| Number of Rows | 100 |
| Number of Columns | 4 |

**Expected schema for the clean data:**

| Column Name | Data Type | Nullable |
|---|---|---|
| channel_name | VARCHAR | NO |
| total_subscribers | INTEGER | NO |
| total_views | INTEGER | NO |
| total_videos | INTEGER | NO |

**Cleaning steps applied:**
1. Remove unnecessary columns — retain only relevant ones
2. Extract YouTube channel names from the first column
3. Rename columns using aliases

### Data Transformation & SQL View

```sql
CREATE VIEW youtuber_db.View_UK_Youtubers_2024 AS

SELECT CAST(substring(NOMBRE, 1, locate('@', NOMBRE) -1) AS CHAR(100)) AS Channel_name,
    total_subscribers,
    total_views,
    total_videos
FROM
    youtuber_db.uk_youtube_db1;
```

---

## 🧪 Testing

### Data Quality Checks

#### 1. Row Count Check
```sql
-- Row Count
SELECT
    count(*)
FROM
    view_uk_youtubers_2024;
```
✅ Result: **100 rows** — as expected
![Row Count Check](https://github.com/Sancti19/Sanctiblaise.github.io/blob/main/assets%20/images/Row_Count_YT.png
)

#### 2. Column Count / Data Type Check
```sql
SELECT
    COLUMN_NAME,
    DATA_TYPE
FROM
    INFORMATION_SCHEMA.COLUMNS
WHERE
    TABLE_SCHEMA = 'youtuber_db'
    AND TABLE_NAME = 'view_uk_youtubers_2024';
```

| Column Name | Data Type |
|---|---|
| Channel_name | varchar |
| total_subscribers | int |
| total_videos | int |
| total_views | bigint |

✅ All data types are appropriate.
![Column Count / Data Type Check](https://github.com/Sancti19/Sanctiblaise.github.io/blob/main/assets%20/images/ColumnCheck.png
)

#### 3. Duplicate Check
```sql
-- DUPLICATE
SELECT DISTINCT
    Channel_name,
    COUNT(*) AS duplicate
FROM
    youtuber_db.view_uk_youtubers_2024
GROUP BY Channel_name
HAVING
    COUNT(*) > 1
```
✅ No duplicates found.
![Duplicate Check](https://github.com/Sancti19/Sanctiblaise.github.io/blob/main/assets%20/images/Duplicate.png)

---

## 📈 Visualization

### Dashboard Results

The Power BI dashboard displays the **Top UK YouTubers in 2024**, including:

- Top 10 channels by subscribers
- Top 10 channels by total views (Treemap)
- Channel Engagement Ratios (scorecards):
  - **1.02M** Avg Views per Video
  - **2.28K** Subscriber Engagement Rate
  - **446.41** Views per Subscriber

### DAX Measures

#### 1. Total Subscribers (M)
```dax
TotalSubscribers (M) =
VAR millions = 1000000
VAR SumofSubscribers = SUM('youtuber_db view_uk_youtubers_2024'[total_subscribers])
VAR TotalSubscribers = DIVIDE(SumofSubscribers, millions)

RETURN TotalSubscribers
```
![DAX- Total Subscribers](https://github.com/Sancti19/Sanctiblaise.github.io/blob/main/assets%20/images/Total%20Subscribers%20PBI.png
)

#### 2. Total Videos
```dax
TotalVideos =
VAR TotalVideos = SUM('youtuber_db view_uk_youtubers_2024'[total_videos])

RETURN TotalVideos
```
![DAX- Total Videos](https://github.com/Sancti19/Sanctiblaise.github.io/blob/main/assets%20/images/Total%20videos.png)

#### 3. Total Views (B)
```dax
TotalViews (B) =
VAR Billions = 1000000000
VAR SumofViews = SUM('youtuber_db view_uk_youtubers_2024'[total_views])
VAR TotalViews = DIVIDE(SumofViews, Billions)

RETURN TotalViews
```

#### 4. Average Views per Video (M)
```dax
Avg_Views_per_Video (M) =
VAR sumofviews = SUM('youtuber_db view_uk_youtubers_2024'[total_views])
VAR sumofvideos = SUM('youtuber_db view_uk_youtubers_2024'[total_videos])
VAR Avg_views_per_video = DIVIDE(sumofviews, sumofvideos, BLANK())
VAR FinalViewsPerVideo = DIVIDE(Avg_views_per_video, 1000000, BLANK())

Return FinalViewsPerVideo
```
![DAX- Average Views Per Video](https://github.com/Sancti19/Sanctiblaise.github.io/blob/main/assets%20/images/Average%20views%20per%20video.png)

#### 5. Views per Subscriber
```dax
Views Per Subscribers =
VAR totalviews = SUM('youtuber_db view_uk_youtubers_2024'[total_views])
VAR totalsubscribers = SUM('youtuber_db view_uk_youtubers_2024'[total_subscribers])
VAR ViewsPerSubscribers = DIVIDE(totalviews, totalsubscribers, BLANK())

RETURN ViewsPerSubscribers
```
![DAX- Views Per Subscriber](https://github.com/Sancti19/Sanctiblaise.github.io/blob/main/assets%20/images/ViewPerSubscriberPBI.png
)

#### 6. Subscriber Engagement Rate
```dax
Subscriber Engagement Rate =
VAR SumOfVideos = SUM('youtuber_db view_uk_youtubers_2024'[total_videos])
VAR SumofSubscribers = SUM('youtuber_db view_uk_youtubers_2024'[total_subscribers])
VAR SubscriberEngagementRate = DIVIDE(SumofSubscribers, SumOfVideos, BLANK())

RETURN SubscriberEngagementRate
```
![DAX- Subscriber Engagement Rate](https://github.com/Sancti19/Sanctiblaise.github.io/blob/main/assets%20/images/Engagement%20per%20subscriber.png
)

---

## 🔍 Analysis

### Findings

#### 1. Top 10 YouTubers by Subscribers

| Rank | Channel Name | Subscribers (M) |
|---|---|---|
| 1 | NoCopyrightSounds | 33.60 |
| 2 | DanTDM | 28.60 |
| 3 | Dan Rhodes | 26.50 |
| 4 | Miss Katy | 24.50 |
| 5 | Mister Max | 24.40 |
| 6 | KSI | 24.10 |
| 7 | Jelly | 23.50 |
| 8 | Dua Lipa | 23.30 |
| 9 | Sidemen | 21.00 |
| 10 | Ali-A | 18.90 |

![Top 10 YouTubers](https://github.com/Sancti19/Sanctiblaise.github.io/blob/main/assets%20/images/Power%20BI%20Desktop%2009_03_2026%2016_27_07.png
)

#### 2. Top 3 Channels by Videos Uploaded

| Rank | Channel Name | Videos |
|---|---|---|
| 1 | GRM Daily | 14,696 |
| 2 | Manchester City | 8,248 |
| 3 | Yogscast | 6,435 |

#### 3. Top 3 Channels by Total Views

| Rank | Channel Name | Total Views (B) |
|---|---|---|
| 1 | DanTDM | 19.78 |
| 2 | Dan Rhodes | 18.56 |
| 3 | Mister Max | 15.95 |

#### 4. Top 3 Channels by Average Views per Video

| Rank | Channel Name | Avg Views per Video (M) |
|---|---|---|
| 1 | Mark Ronson | 322.79 |
| 2 | Jessie J | 59.77 |
| 3 | Dua Lipa | 57.62 |

#### 5. Top 3 Channels by Views per Subscriber Ratio

| Rank | Channel Name | Views per Subscriber |
|---|---|---|
| 1 | GRM Daily | 1,185.79 |
| 2 | Nickelodeon | 1,061.04 |
| 3 | Disney Junior UK | 1,031.97 |

#### 6. Top 3 Channels by Subscriber Engagement Rate

| Rank | Channel Name | Engagement Rate |
|---|---|---|
| 1 | Mark Ronson | 343,000 |
| 2 | Jessie J | 110,416.67 |
| 3 | Dua Lipa | 104,954.95 |

---

### Validation

#### 1. YouTubers with the Most Subscribers
**Campaign idea: Product Placement**

Assumptions:
- Conversion rate: 2%
- Product cost: $5
- Campaign cost (one-time fee): $50,000

| Channel | Avg Views/Video | Potential Units Sold | Potential Revenue | Campaign Cost | Net Profit |
|---|---|---|---|---|---|
| Dan Rhodes | 11.15M | 223,000 | $1,115,000 | $50,000 | **$1,065,000** ✅ |
| NoCopyrightSounds | 6.92M | 138,400 | $692,000 | $50,000 | $642,000 |
| DanTDM | 5.34M | 106,800 | $534,000 | $50,000 | $484,000 |

> 🏆 **Best option from category: Dan Rhodes**

---

#### 2. YouTubers with the Most Videos Uploaded
**Campaign idea: Sponsored Video Series**

Assumptions:
- Conversion rate: 2%
- Product cost: $5
- Campaign cost: 11 videos @ $5,000 each = $55,000

| Channel | Avg Views/Video | Potential Units Sold | Potential Revenue | Campaign Cost | Net Profit |
|---|---|---|---|---|---|
| Yogscast | 710,000 | 14,200 | $71,000 | $55,000 | **$16,000** ✅ |
| GRM Daily | 510,000 | 10,200 | $51,000 | $55,000 | -$4,000 ❌ |
| Manchester City | 240,000 | 4,800 | $24,000 | $55,000 | -$31,000 ❌ |

> 🏆 **Best option from category: Yogscast**

---

#### 3. YouTubers with the Most Views
**Campaign idea: Influencer Marketing**

Assumptions:
- Conversion rate: 2%
- Product cost: $5
- Campaign cost: 3-month contract = $130,000

| Channel | Avg Views/Video | Potential Units Sold | Potential Revenue | Campaign Cost | Net Profit |
|---|---|---|---|---|---|
| Mister Max | 14.06M | 281,200 | $1,406,000 | $130,000 | **$1,276,000** ✅ |
| Dan Rhodes | 11.15M | 223,000 | $1,115,000 | $130,000 | $985,000 |
| DanTDM | 5.34M | 106,800 | $534,000 | $130,000 | $404,000 |

> 🏆 **Best option from category: Mister Max**

---

### Discovery

Key insights from the analysis:

1. **NoCopyrightSounds, Dan Rhodes and DanTDM** are the channels with the most subscribers in the UK
2. **GRM Daily, Man City and Yogscast** are the channels with the most videos uploaded
3. **DanTDM, Dan Rhodes and Mister Max** are the channels with the most views
4. **Entertainment channels** are most useful for broader reach — the channels posting consistently and generating the most engagement are focused on entertainment and music

---

## 💡 Recommendations

1. **Dan Rhodes** is the best YouTube channel to collaborate with if the goal is to **maximize visibility** — this channel has the most YouTube subscribers in the UK
2. Although **GRM Daily, Man City and Yogscast** are regular publishers on YouTube, collaborating with them within current budget caps may not be worth the effort, as the potential ROI is significantly lower compared to other channels
3. **Mister Max** is the best YouTuber to collaborate with for **maximizing reach**, but collaborating with **DanTDM and Dan Rhodes** may be better long-term options given their large subscriber bases and consistently high view counts
4. The **top 3 channels** to form collaborations with are **NoCopyrightSounds, DanTDM and Dan Rhodes**, as they attract the most consistent engagement

### Potential ROI

| Collaboration | Campaign Type | Expected Net Profit |
|---|---|---|
| Dan Rhodes | Product Placement | **$1,065,000** per video |
| Mister Max | Influencer Marketing | **$1,276,000** (3-month contract) |
| DanTDM | Product Placement | **$484,000** per video |
| NoCopyrightSounds | Product Placement | **$642,000** per video |

### Action Plan

Based on the analysis, the **recommended primary partner is Dan Rhodes** for a long-term partnership deal.

**Steps to implement:**

1. Reach out to the teams behind each of these channels, starting with **Dan Rhodes**
2. Negotiate contracts within the budgets allocated to each marketing campaign
3. Kick off the campaigns and track each of their performances against the KPIs
4. Review campaign performance, gather insights, and optimize based on feedback from converted customers and each channel's audience

> Once expected milestones are hit with Dan Rhodes, advance with potential partnerships with **DanTDM, Mister Max and NoCopyrightSounds**.

---

## ✅ Conclusion

This analysis provides a clear, data-driven framework for selecting the right UK YouTube influencers for marketing partnerships. By evaluating channels across subscribers, views, engagement, and projected ROI — rather than popularity alone — the marketing team can make confident, high-return decisions on where to invest their influencer budget.

---

*Analysis conducted using Excel, SQL Server, and Power BI. Data sourced from Kaggle.*
