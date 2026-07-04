# Maven Toys Sales & Inventory Analytics

An end-to-end retail analytics project answering four core business questions for Maven Toys — a 50-store retail chain across Mexico — using **PostgreSQL** for data modeling and analysis, and **Power BI** for an interactive, finding-first dashboard.

---

## Why This Was Built

Most portfolio dashboards report numbers. This one was built to do something different: **every visual answers a specific question, flags what's wrong, and points toward what to do about it.** The goal wasn't just to show *what* happened in the business, but to find where problems exist today and where they're likely to emerge.

The project was scoped around four questions:
1. Which product categories drive the biggest profits — and is this consistent across store locations?
2. Are there seasonal trends or patterns in the sales data?
3. Are we losing sales due to products being out of stock at certain locations?
4. How much money is tied up in inventory, and how long will it last?

---

## Tools Used

| Tool | Purpose |
|---|---|
| **PostgreSQL** | Data modeling, cleaning, and all analytical SQL (CTEs, window logic, normalized aggregation) |
| **Power BI** | Dashboard build, DAX measures, interactive visuals |
| **Excel** | Quick exploratory validation and stakeholder-facing summaries |

---

## Dataset

Six related tables covering an 18-month period (Jan 2022 – Sep 2023) across 50 stores in Mexico:

- **Products** — 35 SKUs across 5 categories (Toys, Electronics, Art & Crafts, Games, Sports & Outdoors)
- **Stores** — 50 locations across 4 location types (Downtown, Commercial, Residential, Airport)
- **Inventory** — current stock-on-hand snapshot per store-product
- **Sales** — 829,262 transaction-level records
- **Calendar** — daily date reference table

**A key data constraint shaped the methodology throughout this project:** the data only runs through September 2023, meaning January–September have two years of data while October–December have only one. Every time-based analysis in this project (monthly trends, weekday trends) uses **averages normalized across years**, not raw totals, to avoid an artificial skew toward the months with more data.

---

## Q1 — Which Categories Drive the Most Profit?

**Finding:** Toys drives the highest total profit ($1.08M) but has the weakest margin of any category (21.2%). Electronics, by contrast, earns nearly as much profit ($1.00M) from less than half the revenue — a 44.6% margin, more than double Toys'. This pattern holds consistently across every store location type (Airport, Commercial, Downtown, Residential) — it's structural to the category, not a location-specific issue.

At the product level: **Colorbuds** (Electronics) is the standout — the top-selling product by volume (104K units) *and* one of the highest-margin products (53.4%). **Jenga** (Games) has the highest margin in the entire catalog (70.1%) despite modest volume — a different path to profitability built on pricing power rather than scale. **Lego Bricks** is the biggest volume-margin mismatch: $2.39M in revenue but only 12.5% margin.

**Action:** Investigate Toys' pricing and cost structure company-wide (the issue isn't location-specific). Study what makes Colorbuds and Jenga work — their models could inform a fix for lower-margin products.

**Impact:** Even a modest margin improvement on Toys' $5M+ revenue base would meaningfully lift company-wide profit, more than the same percentage-point gain would on a smaller category.

---

## Q2 — Are There Seasonal Patterns in Sales?

**Finding:** Not every category follows the same calendar. **Toys** peaks in spring (March–June) and declines through August — its own distinct cycle. **Art & Crafts** and **Games** are true gift categories, spiking sharply only in December. **Sports & Outdoors** shows a dual peak (summer and December). **Electronics** stays remarkably flat all year, with almost no seasonality.

At the weekly level: **Saturday** ($1.43M avg) and **Friday** ($1.38M avg) are consistently the strongest revenue days — roughly 60% higher than **Monday**, the weakest ($766K).

**Action:** Replace a single company-wide seasonal plan with category-specific inventory timing — stock Toys ahead of spring, Art & Crafts and Games ahead of December, and let Electronics run on a flat, steady schedule. Increase staffing and stock availability ahead of Friday and Saturday specifically.

**Impact:** Matching replenishment timing to each category's actual demand cycle reduces both stockout risk during peaks and excess holding costs during troughs.

---

## Q3 — Are We Losing Sales to Stockouts?

**The challenge:** the inventory table is a current snapshot with no stockout history, so exact historical lost-sales figures aren't directly measurable. **The workaround:** for every store-product combination currently out of stock, this project calculates that item's historical average daily sales rate, then estimates the revenue likely being lost *per day* the stockout continues — a transparent, defensible proxy rather than a precise historical total.

**Finding:** Toys and Art & Crafts account for roughly 70% of an estimated **$10,266/day** in company-wide lost revenue from stockouts. Toys alone has both the highest stockout rate (7.4%) and the weakest margin — a category losing money two ways at once.

A sharper cut by location revealed a counter-intuitive result: **Downtown loses the most in raw dollars** ($5,901/day), but **Residential stores lose the highest share of their sales** (7.28% vs. Downtown's 3.97%) — a proportionally bigger operational problem, even though it's less visible in absolute terms.

**Action:** Prioritize replenishment fixes for Toys and Art & Crafts first. Don't judge stockout severity by raw dollars alone — Residential's proportional exposure deserves equal attention to Downtown's.

**Impact:** Even halving stockouts in Toys and Art & Crafts alone could recover an estimated $1.3M+ annually.

---

## Q4 — How Much Money Is Tied Up in Inventory, and How Long Will It Last?

**Finding:** Roughly **$300,000** is currently tied up in inventory (at cost) across all 50 stores. At current sell-through rates, every store carries less than 2 days of supply — an extremely lean turnover model that helps explain the stockout pattern found in Q3.

A closer look revealed that stockout rate isn't purely explained by buffer size: **Toys has more days of supply (1.28) than Electronics (0.93), yet stocks out far more often** (7.38% vs. 2.68%). This points to demand volatility, not just inventory thinness, as the real driver — Toys' sharper seasonality likely makes it harder to forecast accurately even with a reasonable buffer.

Two additional findings emerged from this page: **Toys carries the highest inventory value ($99,861) and the highest stockout loss** — a concentration of both capital and risk in one category. **Electronics**, meanwhile, has the fewest products and the lowest inventory value, yet the highest margin and near-top revenue — an efficient, underexpanded category worth considering for growth.

**Action:** Improve demand forecasting specifically for Toys rather than uniformly increasing inventory. Audit which specific Toys SKUs are overstocked versus understocked. Consider expanding the Electronics product range.

**Impact:** Better-targeted inventory allocation for Toys directly reduces both tied-up capital and stockout losses simultaneously, since both problems are concentrated in the same category.

---

## Bonus Finding — Store Location Performance

**Finding:** Downtown drives the most total revenue (56.9% of the business) largely because it has the most stores (29 of 50) — not necessarily because each store outperforms. On a per-store basis, **Airport locations perform best**: $429,908 revenue per store (vs. Downtown's $283,434) and the highest margin (29.31%), despite having only 3 locations company-wide. Two of the top three most profitable individual stores in the entire chain are Airport locations.

**Action:** Use the Airport model to guide future store expansion decisions rather than defaulting to Downtown-style locations.

**Impact:** Airport's demonstrated per-store efficiency suggests untapped, profitable growth potential that isn't yet reflected in store count.

---

## Final Recommendation

**Prioritize Toys.** It is simultaneously the lowest-margin category, the most stockout-prone, and the largest concentration of tied-up inventory capital — fixing either its pricing strategy or its replenishment process would have the single largest company-wide impact of any change available in this analysis.

---

## Dashboard Structure

| Page | Covers |
|---|---|
| **Overview** | Company-wide KPIs, profit by category, revenue share by location, seasonal trend |
| **Profit** | Product-level profit and margin detail, top performers, category risk/opportunity findings |
| **Seasonality** | Category-level seasonal patterns, weekday revenue heatmap |
| **Stock & Inventory** | Stockout rate by category, days of supply by category, inventory value vs. lost revenue |
| **Store & Location** | Revenue/margin by location type, top-performing individual stores |

---

## Methodology Notes & Limitations

- **Uneven date range (Jan 2022–Sep 2023):** all monthly and weekday trends use averages normalized across years, not raw sums, to avoid skew from months with more historical data.
- **Stockout loss estimates** assume a product would have sold at its historical average daily rate had it been in stock — a reasonable proxy, not a guaranteed lost-sales figure, since real customers may substitute another product rather than not purchasing at all.
- **Inventory snapshot:** `inventory` reflects a single point in time with no historical stockout dates, which is why lost revenue is expressed as a *daily* estimate rather than a total historical figure.
- **"Maven Toys"** is the retail chain's brand name — all 50 stores sell products across all 5 categories; "Toys" refers specifically to the product category, not the store type.

---

*Built by Anas Khan — [GitHub](https://github.com/Akhan33-10) · [LinkedIn](https://linkedin.com/in/anas-khan-285415403)*
