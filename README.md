# 💊 Cortonis Pharma — Sales Performance Dashboard

![Figma](https://img.shields.io/badge/Figma-F24E1E?style=for-the-badge&logo=figma&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-4A4A4A?style=for-the-badge&logo=microsoftexcel&logoColor=white)

> A commercial performance dashboard for a fictional pharmaceutical company — built to answer the questions a sales manager actually asks, with a UX-first approach from Figma wireframe to deployed report.

---

## Status

| Page | Status |
|------|--------|
| Executive Overview | ✅ Complete |
| Territory & Geographic Performance | ✅ Complete |
| Product & Therapeutic Class | ✅ Complete |
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

**Foresight — Pharmaceutical Manufacturing Company's Wholesale-Retail Data**
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

> Note on Poland data: sales data for Poland is available for 2018 only. YoY variance indicators are intentionally hidden for Poland to avoid misleading comparisons.

---

## Page 1 — Executive Overview

### What it answers
- How is the business performing overall — in sales, volume, orders, pricing, and customer count?
- Is performance improving or declining vs. the previous year?
- What does the sales trend look like across the year, and how does it vary by quarter?
- Which territories, products, channels, or teams are driving the most value?

### Screenshot

![Executive Overview](Screenshots/Executive_Overview.PNG)

### KPI Cards

Five top-level metrics, each showing the current period value alongside both the previous year's absolute value and the variance in absolute and percentage terms (color-coded green/red):

| KPI | Description |
|-----|-------------|
| **Sales** | Total revenue for the selected period |
| **Units Sold** | Total quantity sold |
| **Orders** | Number of distinct transactions |
| **Price / Unit on Average** | Average selling price per unit |
| **Customers** | Number of distinct active customers |

Each card displays: current value · last year value · absolute variance · % variance. Showing both absolute and relative variance is a deliberate choice — on billion-dollar figures, a "-16%" means more when paired with "-$575.96M". YoY variance adapts automatically to the selected period using `PARALLELPERIOD` DAX logic.

### Trend Chart — Current Year vs. Last Year + Field Parameter (Metric)

The chart overlays two series simultaneously — current period and same period last year — making period-over-period trends immediately visible without any additional interaction. A dropdown field parameter lets the user switch the displayed metric across all five KPIs:

```
Sales → Units Sold → Orders → Price/Unit → Customers
```

Switching the metric updates both the trend chart and the distribution matrix simultaneously — a single selection drives the entire page.

### Distribution Matrix — Dual Field Parameters

The matrix on the right provides a ranked breakdown of the selected metric. It uses **two independent field parameters**:

**Parameter 1 — Metric** (shared with the trend chart)
Switches the value column between Sales, Units Sold, Orders, Price/Unit, and Customers.

**Parameter 2 — Dimension**
A dropdown switches the breakdown axis between:
- Locations (Country → Region)
- Product Class → Product
- Channel → Sub-channel
- Sales Team → Sales Rep

The matrix displays three columns: the absolute value, a data bar for visual ranking, and a **Var% vs. Last Year** column — color-coded green/red — so performance context is visible directly in the breakdown without needing a separate visual.

This dual-parameter architecture means the user can explore any metric across any dimension from a single page — without navigating away or duplicating report pages.

### Search Bar

A cross-dimension search bar filters the entire page by matching against a concatenated column that combines Channel, Sub-channel, Country, City, Product Class, Product Name, Sales Team, and Manager into a single searchable string per row.

### Slicers

Six synchronized slicers on the left sidebar filter all visuals simultaneously:

`Date` · `Product` · `Channel` · `Country` · `Distributor` · `Sales Team`

A **Clear All** button resets all slicers via a named bookmark — one click returns the page to its default state.

### UX Details

- **Personalized greeting** — `USERPRINCIPALNAME()` renders the logged-in user's name and email in the top-right header
- **Date range slicer** — a dual-handle slider with explicit start/end dates gives precise period control
- **Last updated timestamp** — displayed in the sidebar for data governance transparency
- **Help links** — sidebar footer includes How to use, Contact us, and About
- **Custom navigation** — page navigation via buttons rather than native Power BI tabs
- **Tooltip pages** — hovering over KPI cards and chart data points surfaces a custom tooltip page with contextual detail
- **Author signature** — "Developed by Guillaume Pien" displayed in the report footer

---

## Page 2 — Territory & Geographic Performance

### What it answers
- Where are we growing and where are we losing ground?
- How do Germany and Poland compare across all key metrics?
- Which regions and cities are the strongest performers — and which are declining?
- Which cities are high-volume but low-growth (defend), and which are low-volume but high-growth (accelerate)?

### Screenshot

![Territory Performance](Screenshots/Territory_Performance.PNG)

### Country KPI Cards

Two dedicated cards — one per market — display all five metrics side by side for an immediate Germany vs. Poland comparison:

| Metric | Germany | Poland |
|--------|---------|--------|
| Sales | ✅ with YoY variance | ✅ (no YoY — 2018 only) |
| Units Sold | ✅ with YoY variance | ✅ |
| Price / Unit | ✅ with YoY variance | ✅ |
| Orders | ✅ with YoY variance | ✅ |
| Active Customers | ✅ with YoY variance | ✅ |

YoY variance indicators are suppressed for Poland where prior-year data is unavailable — displaying "--" rather than a misleading 0% or blank.

### Sales by Location Map

Bubble map showing sales volume by city across Poland and Germany. Bubble size is proportional to sales — the spatial distribution of revenue is immediately visible, with Germany showing significantly higher density and concentration than Poland.

### Distribution Matrix

Same dual field parameter architecture as Page 1 — metric and dimension both switchable independently. Defaults to Locations (Country → Region) with Sales and Var% Sales.

### Sales vs Growth YoY% per City — Quadrant Scatter Plot

The analytical centrepiece of this page. Each bubble represents a city, plotted on:
- **X axis** — absolute Sales volume
- **Y axis** — YoY Growth %

**Quadrant logic — BCG Matrix framework:**

| Quadrant | Sales | Growth | Label | Strategic implication |
|----------|-------|--------|-------|----------------------|
| Top right | High | High | ⭐ Stars | Protect and invest |
| Top left | Low | High | ❓ Question Marks | Evaluate and accelerate |
| Bottom right | High | Low | 🐄 Cash Cows | Defend and harvest |
| Bottom left | Low | Low | 🐕 Dogs | Review and deprioritize |

**Key technical details:**
- Quadrant dividers are **dynamic median lines** — recalculated via DAX `MEDIANX` each time a filter is applied. When the user filters by Sales Team or Product, the medians shift to reflect that subset, keeping the quadrant analysis contextually relevant
- Quadrant background colors (blue = positive, pink = negative zones) make the four segments immediately readable without requiring the user to interpret axis values
- **Zoom sliders** on both axes allow the user to isolate the dense city cluster in the $0–$10M range without losing the outliers visible at full scale
- Quadrant labels (Stars, Question Marks, Dogs, Cash Cows) are displayed in the chart legend using BCG Matrix terminology — familiar to any commercial audience

---

## Page 3 — Product Mix & Therapeutic Class

### What it answers
- What are we selling, and is the mix shifting across quarters?
- Which therapeutic classes drive the most revenue — and are they growing?
- Which classes have the strongest seasonal patterns, and when do they peak?

### Screenshot

![Product Mix](./Screenshots/Product_Mix.PNG)

### Product Class Ranks — Ribbon Chart with Field Parameter

A ribbon chart showing the ranking and relative volume of all 6 therapeutic classes across quarters. The crossing of ribbons signals a ranking change between classes — immediately visible without reading the axis values.

A dropdown field parameter lets the user switch the ranking metric:
```
Sales → Units Sold → Orders → Price/Unit → Customers
```

Switching the metric rerankings all classes dynamically — the same interaction pattern as Pages 1 and 2, keeping the UX consistent across the dashboard.

### Distribution By Product — Matrix with Drill-Down

Same dual field parameter architecture as Pages 1 and 2:

**Parameter 1 — Metric:** Sales, Units Sold, Orders, Price/Unit, Customers
**Parameter 2 — Dimension:** defaults to Product Class, drill-down to Product Name

The matrix displays absolute value, data bar, and Var% YoY per class — giving immediate context on both volume and trend without requiring a separate visual. Data bars use cyan (`#58FFE6`) to distinguish them from variance indicators.

### Seasonality Analysis — Small Multiples with Confidence Intervals

The most technically sophisticated visual in the dashboard. Six line charts in a 2×3 small multiples grid — one panel per therapeutic class — showing how normalized monthly sales evolve across the year relative to the annual average.

**The Seasonality Index**

The Y axis displays a normalized index rather than raw sales values:

```
Seasonality_Index = Monthly Total Sales / Average Monthly Sales (annual)
```

A value of 1.0 means the month is exactly average. A value of 1.5 means 50% above average. This normalization makes the 6 classes directly comparable regardless of their very different absolute volumes — Analgesics at $692M and Antimalarial at $452M show on the same scale.

A dashed reference line at Y=1.0 (labeled "Avg") materializes the baseline on each panel, making deviations immediately readable without requiring the user to interpret axis values.

**95% Confidence Intervals**

Each panel shows a shaded band around the index line representing a 95% confidence interval — calculated as:

```
IC = Index ± 1.96 × (StdDev / √N)
```

Where StdDev is the standard deviation of the seasonality index across all products within the class for that month, and N is the number of distinct products. A narrow band means all products in the class share a similar seasonal pattern. A wide band means high dispersion — some products peak while others trough in the same month.

**Key patterns visible in the data:**

| Class | Peak months | Interpretation |
|-------|-------------|----------------|
| Analgesics | June – August | Estival peak — sports injuries, outdoor activity |
| Antibiotics | January – February | Winter peak — respiratory infections |
| Antimalarial | April & October | Two peaks — spring and autumn travel seasons |
| Antipiretics | February & November | Winter peaks — fever and infections |
| Antiseptics | Relatively flat | Low seasonality — regular usage year-round |
| Mood Stabilizers | Complex oscillations | Multiple peaks — consistent with seasonal depression literature |

**Technical implementation:**

The confidence interval measures use `REMOVEFILTERS` rather than `ALLEXCEPT` to correctly preserve the month context across both `Month Number` and `Month_Name_Short` columns simultaneously:

```dax
Seasonality_Index =
VAR TotalThisMonth =
    CALCULATE(
        SUM(Fact_Sales[Sales]),
        REMOVEFILTERS('Calendar'[Date]),
        REMOVEFILTERS('Calendar'[Year]),
        REMOVEFILTERS('Calendar'[Month_Name_Short])
    )
VAR TotalAllYear =
    CALCULATE(
        SUM(Fact_Sales[Sales]),
        REMOVEFILTERS('Calendar')
    )
VAR NbMonths =
    CALCULATE(
        DISTINCTCOUNT('Calendar'[Month Number]),
        REMOVEFILTERS('Calendar')
    )
VAR AvgMonthlyTotal = DIVIDE(TotalAllYear, NbMonths)
RETURN
    DIVIDE(TotalThisMonth, AvgMonthlyTotal)
```

```dax
Seasonality_Upper_95 =
VAR AvgIndex = [Seasonality_Index]
VAR StdDev =
    CALCULATE(
        STDEVX.P(
            VALUES(Fact_Sales[Product Name]),
            CALCULATE([Seasonality_Index])
        ),
        REMOVEFILTERS('Calendar'[Date]),
        REMOVEFILTERS('Calendar'[Year]),
        REMOVEFILTERS('Calendar'[Month_Name_Short])
    )
VAR N =
    CALCULATE(
        DISTINCTCOUNT(Fact_Sales[Product Name]),
        REMOVEFILTERS('Calendar'[Date]),
        REMOVEFILTERS('Calendar'[Year]),
        REMOVEFILTERS('Calendar'[Month_Name_Short])
    )
VAR MarginOfError = 1.96 * DIVIDE(StdDev, SQRT(N))
RETURN
    AvgIndex + MarginOfError
```

`Seasonality_Lower_95` is identical with `AvgIndex - MarginOfError` in the RETURN.

---

## Design Approach

The dashboard was **wireframed in Figma** before any Power BI development began. Layout, navigation flow, color palette, KPI card structure, and slicer placement were all defined at the wireframe stage — ensuring design decisions were intentional rather than reactive.

**Why UX matters in BI**

Users today are surrounded by polished consumer apps and websites. When a new internal tool doesn't meet that same standard of experience, adoption suffers — not because the data is wrong, but because people don't have time to relearn how to navigate yet another unfamiliar interface.

This dashboard is built on the premise that a BI report should feel as intuitive as the apps people already use daily. That means consistent navigation, a clear visual hierarchy, and — critically — each page designed around a single audience and a single question. A field manager shouldn't have to scroll past executive-level content to find their territory data. A sales leader shouldn't be overwhelmed by rep-level detail on a summary page. When the right information reaches the right person without friction, adoption follows naturally.

**Typography**

| Role | Font |
|------|------|
| Titles & headers | Trebuchet MS |
| Body & data labels | Segoe UI |

**Color palette**

| Color | Hex | Usage |
|-------|-----|-------|
| Emerald Green | `#03DE74` | Positive variance, growth indicators |
| Navy | `#0B1F52` | Sidebar background, primary dark elements |
| Cyan | `#58FFE6` | Data bars, chart fills, accent visuals |
| Mint | `#55FFCC` | Secondary accent |
| Red | `#D7263D` | Negative variance, decline indicators |

The palette is intentionally tight — two neutrals (navy, white) and three accent colors with clear semantic roles. Green and red are reserved exclusively for variance indicators; cyan and mint handle all data visualization. This avoids the common mistake of using color decoratively, which creates visual noise without adding information.

**Design principles applied:**
- Defined color semantics — every color has a single purpose and never appears outside that role
- One question per page — each page has a defined analytical purpose and a defined audience
- App-like experience — custom button navigation, personalized header, feedback footer
- Information density calibrated by audience — executive page prioritizes scannable KPIs; detail pages support deeper exploration

---

## Key DAX Measures

**Dynamic YoY variance**
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

**Dynamic median for scatter quadrant lines**
```dax
Median Sales =
MEDIANX(
    VALUES(Data[City]),
    CALCULATE([Total Sales])
)
```

**Germany Sales (country-filtered)**
```dax
Germany Sales =
CALCULATE([Total Sales], Data[Country] = "Germany")
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
| `Pharma_Sales_Dashboard_V0_1.pbix` | Power BI report file |
| `Screenshots/` | Dashboard page screenshots |

---

## Data Source

**Foresight — Pharmaceutical Manufacturing Company's Wholesale-Retail Data**
[https://foresightbi.com.ng/practice-data/3-datasets-for-your-portfolio/](https://foresightbi.com.ng/practice-data/3-datasets-for-your-portfolio/)

---

*Built as Project 1 of a two-part pharma commercial analytics series. [Project 2](https://github.com/gpn64/pharma-sales-analytics) extends the analysis with Python-based customer segmentation, territory underperformance modeling, and sales forecasting.*
