# Google-vs-Facebook-Ad-Campaign-Analysis
Excel dashboard analysing Google Ads and Facebook Ads campaign performance, including CPC, CTR, regression analysis, and data-driven recommendations.

## 1. Business Problem

A marketing team runs paid campaigns on two platforms: Google Ads (search) and Facebook Ads (social). With a limited budget, they need to know:

- Which platform and which segments (keyword type, device, age, creative, audience) deliver traffic most efficiently?
- Where is money being spent at a high cost per click?
- What should be tested or changed to get more from the same budget?

**Objectives:** carry out exploratory data analysis, build a simple model to evaluate performance drivers, and summarise the findings in a professional dashboard.

---

## 2. The Data

16,834 daily campaign rows with 16 fields. Dataset source: [add link here]. Check the license before re-hosting the data.

| | Google Ads | Facebook Ads |
|---|---|---|
| Period | 16 Oct 2019 - 7 Jul 2020 | 16 Dec 2019 - 20 Mar 2020 |
| Campaign type | Search | Conversion |
| Segments | Subchannel (Brand, Competitor, Generic), device, age | Age, creative type (Image, Carousel), audience (1, 2, 3) |

**Key fields:** `campaign_platform`, `subchannel`, `audience_type`, `creative_type`, `device`, `age`, `spends`, `impressions`, `clicks`, `link_clicks`.

**Metric definitions** (all ratios are calculated from totals, not averages of daily values):

- **CTR** = clicks / impressions
- **CPC** = spend / clicks
- **Cost per link click** = spend / link clicks (Facebook only; Google has no link-click data)

**Data quality notes:**

- No conversion field exists, so cost per acquisition, conversion rate and ROAS cannot be calculated. The "CPA" field in the original pivot is really cost per link click and is labelled that way throughout.
- 205 exact duplicate rows (total spend $3.58, left in).
- 30 Google rows have clicks above impressions.
- 546 Facebook rows have blank link clicks (0.3% of Facebook spend).
- Facebook has no device recorded.

---

## 3. Results

### Dashboard


Overview 

<img width="773" height="1001" alt="image" src="https://github.com/user-attachments/assets/f03564b8-99f4-4e83-ad87-da2602c0b97a" />


Google Ads 

<img width="775" height="1007" alt="image" src="https://github.com/user-attachments/assets/80a39c9f-319a-410e-b011-a48b3dfb0336" />


Facebook Ads 

<img width="777" height="1002" alt="image" src="https://github.com/user-attachments/assets/619468b9-cd35-4cef-8e80-b431a255dcae" />




<!-- Add screenshots of each dashboard page to the images/ folder. -->

### Headline numbers

| | Google Ads | Facebook Ads |
|---|---|---|
| Spend | $1,939,003 | $564,116 |
| Impressions | 776,893 | 4,070,612 |
| Clicks | 124,065 | 77,569 |
| CTR | 15.97% | 1.91% |
| CPC | $15.63 | $7.27 |

### Key findings

1. **Google took 77% of spend; Facebook delivered 84% of impressions** at less than half the CPC. The platforms serve different roles (search captures existing demand), so they are not a like-for-like comparison.
2. **Generic keywords take 52% of Google spend at a $23.12 CPC**, vs $9.55 for Brand. Over the same Dec-Mar window when all three ran, Brand's CPC was $9.53 vs $22.60 (Generic) and $24.24 (Competitor).
3. **Mobile takes 69% of Google spend at a $14.24 CPC**, vs $20.08 on desktop. Desktop has the higher CTR (18.2% vs 15.4%).
4. **Facebook efficiency fell as spend scaled.** Monthly spend rose from $57k (Jan) to $241k (Feb). CPC went from $3.86 to $8.95 by March, and cost per link click from $8.41 to $20.88.
5. **Carousel costs more per click than Image ($9.28 vs $7.12) but less per link click ($13.41 vs $16.27)**, on only 9% of spend.
6. **Google spend fell from about $590k a month (Dec-Jan) to about $15k from April**, while CPC fell from $21.92 to $2.78.

### Recommendations

- Review Generic search terms and test moving a small share of Generic budget to Brand.
- Check mobile vs desktop results by subchannel before changing device budgets.
- Scale Facebook budgets in steps and pause increases if costs pass an agreed limit.
- Run controlled creative and audience tests over the same period with equal budgets.

### Clicks model

A daily regression of clicks on spend and impressions (`LINEST` in Excel, see `analysis/Ad-analysis-additions.xlsx`):

| | Both inputs (R²) | Impressions only | Spend only |
|---|---|---|---|
| Google Ads | 0.98 | 0.978 | 0.86 |
| Facebook Ads | 0.90 | 0.895 | 0.87 |

Impressions explain nearly all of the variation in clicks. This is a descriptive model on autocorrelated daily data, not a validated forecast. Conversions are not in the dataset, so clicks is the target.

---

## 4. Next Steps and Limitations

### Limitations

- **No conversions or sales data**, so results reflect click costs and engagement, not business outcomes.
- **Platforms are not directly comparable.** Google and Facebook cover different periods and campaign types. Over the shared period (16 Dec - 20 Mar), Google's CPC was $16.37.
- **Unequal time windows.** Competitor and Generic keywords ran only 3 Dec - end of March, and Audiences 2 and 3 ran mostly in the cheaper Dec-Jan months, so some comparisons reflect timing.
- **Small segments.** Facebook ages 55-64 total $878, and Audience 3 had $4.7k of spend.
- **Data quality issues** listed in section 2 (duplicates, blank link clicks, clicks above impressions).

### Next steps

- Add conversion data to calculate true cost per acquisition, conversion rate and ROAS, and re-rank the segments on outcomes.
- Run controlled A/B tests (same period, equal budgets) for Generic vs Brand, Carousel vs Image, and the Facebook audiences.
- Test a gradual Facebook budget ramp and track the point where cost per link click rises.
- Extend the model with more features (day of week, subchannel, device) and validate it on held-out weeks, or move the analysis to Python for a proper train/test split.
- Investigate why Google spend dropped sharply from April and whether results held up.

---

## Repository

```
├── README.md
├── data/Ad-data-Assignment-1.csv           # raw data (check license before including)
├── dashboard/
│   ├── ad-campaign-dashboard.xlsx          # Excel dashboard (pivot tables and charts)
│   └── ad-campaign-dashboard.pdf           # PDF export
├── analysis/Ad-analysis-additions.xlsx     # KPI tables, Facebook breakdowns, clicks regression
└── images/                                 # dashboard screenshots
```

**Tools:** Microsoft Excel (PivotTables, calculated fields, charts, `SUMIFS`, `LINEST`).

**Author:** [Your name] - [LinkedIn or email]
