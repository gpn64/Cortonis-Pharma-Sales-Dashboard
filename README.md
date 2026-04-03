# 💊 Cortonis Pharma — Sales Performance Dashboard

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-4A4A4A?style=for-the-badge&logo=microsoftexcel&logoColor=white)
![Figma](https://img.shields.io/badge/Figma-F24E1E?style=for-the-badge&logo=figma&logoColor=white)

## Status

| Page | Status |
|------|--------|
| Executive Overview | ✅ Complete |
| Territory & Geographic Performance | 🔄 In progress |
| Product & Therapeutic Class | 🔄 In progress |
| Channel & Customer Analysis | 🔄 In progress |
| Sales Rep & Team Performance | 🔄 In progress |

---

## The Context

**Cortonis Pharma** is a fictional pharmaceutical company operating across Poland and Germany. The dataset covers 254,000 sales transactions across 4 years (2017–2020), including product-level detail, customer and distributor information, channel breakdown, geographic data, and sales team hierarchy.

The dashboard is designed for two audiences:
- **Sales leadership** — executive overview, trend monitoring, high-level KPIs
- **Territory and field managers** — granular performance by rep, territory, product, and channel

---

## Data Source

**Kaggle — Pharmaceutical Sales Dataset**
254,082 transactions · Poland & Germany · 2017–2020

| Field | Description |
|-------|-------------|
| `Distributor` | Wholesaler name |
| `Customer Name` | Pharmacy or hospital name |
| `City` / `Country` | Customer location |
| `Channel` | Hospital or Pharmacy |
| `Sub-channel` | Private, Retail, Institution, Government |
| `Product Name` | Drug name |
| `Product Class` | Therapeutic class (Antibiotics, Analgesics, Mood Stabilizers, Antiseptics, Antipyretics, Antimalarial) |
| `Quantity` / `Price` / `Sales` | Transaction volume and value |
| `Month` / `Year` | Transaction period |
| `Name of Sales Rep` | Rep who facilitated the sale |
| `Manager` / `Sales Team` | Team hierarchy (Alfa, Bravo, Charlie, Delta) |

---

## Page 1 — Executive Overview

### What it answers
- How is the business performing overall — in sales, volume, orders, pricing, and customer count?
- Is performance improving or declining vs. the previous year?
- What does the sales trend look like across the year, and how does it vary by quarter?
- Which territories, products, channels, or teams are driving the most value?

### Screenshots

![Executive Overview](./screenshots/executive_overview.png)

### KPI Cards

Five top-level metrics, each showing the current period value and YoY variance (color-coded green/red):

| KPI | Description |
|-----|-------------|
| **Sales** | Total revenue for the selected period |
| **Units Sold** | Total quantity sold |
| **Orders** | Number of distinct transactions |
| **Price / Unit on Average** | Average selling price per unit |
| **Customers** | Number of distinct active customers |

YoY variance is dynamic — it adapts automatically to the selected period (year, quarter, or month) using `PARALLELPERIOD` DAX logic.

### Trend Area Chart — Field Parameter (Metric)

The area chart displays the sales trend across the selected period. A dropdown field parameter lets the user switch the displayed metric across all five KPIs:

```
Sales → Units Sold → Orders → Price/Unit → Customers
```

Switching the metric updates both the area chart and the distribution matrix simultaneously — a single selection drives the entire page.

### Distribution Matrix — Dual Field Parameters

The matrix on the right provides a ranked breakdown of the selected metric. It uses **two independent field parameters**:

**Parameter 1 — Metric** (shared with the area chart)
Switches the value column between Sales, Units Sold, Orders, Price/Unit, and Customers.

**Parameter 2 — Dimension**
Switches the breakdown axis between:
- Country → Region
- Product Class → Product
- Channel → Sub-channel
- Sales Team → Sales Rep

This dual-parameter architecture means the user can explore any metric across any dimension from a single page — without navigating away or duplicating report pages.

### Search Bar

A cross-dimension search bar filters the entire page by matching against a concatenated column that combines Channel, Sub-channel, Country, City, Product Class, Product Name, Sales Team, and Manager into a single searchable string per row.

### Slicers

Six synchronized slicers on the left sidebar filter all visuals simultaneously:

`Date` · `Product` · `Channel` · `Country` · `Distributor` · `Sales Team`

A **Clear All** button resets all slicers via a named bookmark — one click returns the page to its default state.

### UX Details

- **Personalized greeting** — `USERPRINCIPALNAME()` renders the logged-in user's name and email in the top-right header
- **Last updated timestamp** — displayed in the sidebar for data governance transparency
- **Feedback & help links** — sidebar footer includes Send Feedback, FAQ, and About this Dashboard
- **Custom navigation** — page navigation via buttons rather than native Power BI tabs, consistent with the overall app-like experience
- **Tooltip pages** — hovering over KPI cards and area chart data points surfaces a custom tooltip page with contextual detail

---

## Design Approach

The dashboard was **wireframed in Figma** before any Power BI development began. Layout, navigation flow, color palette, KPI card structure, and slicer placement were all defined at the wireframe stage — ensuring design decisions were intentional rather than reactive.

**Why UX matters in BI**

Users today are surrounded by polished consumer apps and websites. When a new internal tool doesn't meet that same standard of experience, adoption suffers — not because the data is wrong, but because people don't have time to relearn how to navigate yet another unfamiliar interface.

This dashboard is built on the premise that a BI report should feel as intuitive as the apps people already use daily. That means consistent navigation, a clear visual hierarchy, and — critically — each page designed around a single audience and a single question. A field manager shouldn't have to scroll past executive-level content to find their territory data. A sales leader shouldn't be overwhelmed by rep-level detail on a summary page. When the right information reaches the right person without friction, adoption follows naturally.

**Design principles applied:**
- Single accent color (cyan/teal) against a dark navy sidebar and light content area — creates visual hierarchy without noise
- One question per page — each report page has a defined analytical purpose and a defined audience; no page tries to answer everything
- App-like experience — custom button navigation, personalized header, feedback footer — the dashboard behaves like a product, not a report
- Information density calibrated by audience — executive page prioritizes scannable KPIs; detail pages support deeper exploration

---

## Key DAX Measures

**Dynamic YoY variance (adapts to selected granularity)**
```dax
Sales YoY % =
VAR CurrentSales = [Total Sales]
VAR PreviousSales =
    CALCULATE(
        [Total Sales],
        PARALLELPERIOD('Calendar'[Date], -1, YEAR)
    )
RETURN
    DIVIDE(CurrentSales - PreviousSales, PreviousSales)
```

**Customers (distinct count)**
```dax
Customers =
DISTINCTCOUNT(Data[Customer Name])
```

**Average Price per Unit**
```dax
Avg Price per Unit =
DIVIDE([Total Sales], [Total Units])
```

---

## Files

| File | Description |
|------|-------------|
| `Pharma_Sales_Dashboard.pbix` | Power BI report file |
| `screenshots/` | Dashboard page screenshots |

---

## Data Source

**Foresight — Pharmaceutical Manufacturing Company’s Wholesale-Retail Data**
[[https://foresightbi.com.ng/practice-data/3-datasets-for-your-portfolio/](https://foresightbi.com.ng/practice-data/3-datasets-for-your-portfolio/)]

---

*Built as Project 1 of a two-part pharma commercial analytics series. [Project 2](https://github.com/gpn64/pharma-sales-analytics) extends the analysis with Python-based customer segmentation, territory underperformance modeling, and sales forecasting.*
