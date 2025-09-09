# operation_olist, a joint data-visualisation project
---
datasets from: https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce


A **comprehensive exploration** of **Olist**’s e-commerce business in Brazil, covering topics such as **sales drivers**, **repeat purchases**, and **online operational factors** (e.g., listing optimization, payment options).

---

## Table of Contents
1. [Project Context](#project-context)  
2. [Data Sources & Overview](#data-sources--overview)  
3. [Data Cleaning & Preprocessing](#data-cleaning--preprocessing)  
4. [Key Insights & Derived Files](#key-insights--derived-files)  
5. [Technical Details & Libraries](#technical-details--libraries)  
6. [Project Structure](#project-structure)  
7. [Acknowledgments & Disclaimer](#acknowledgments--disclaimer)

---

## Project Context
**Olist** serves as a channel for Brazilian merchants to sell on multiple marketplaces. Between **2016–2018**, it experienced notable growth within an expanding e-commerce market. At the same time, **logistical complexities** (e.g., long distances, varying infrastructure) introduced challenges:

- **Goal**: Pinpoint **what** drives sales (categories, locations, promotions), **why** customers repurchase (delivery reliability, freight costs), and **how** to optimize online operations (listing design, payment methods).

---

## Data Sources & Overview
This project integrates multiple datasets (commonly obtained from the **public Olist dataset** on Brazilian e-commerce):

1. **`orders.csv`**  
   - Order-level details: IDs, timestamps, status, estimated vs. actual delivery time.  
2. **`products.csv`**  
   - Category, product dimensions, number of images (`product_photos_qty`), name length, etc.  
3. **`order_reviews.csv`**  
   - Customer review ratings, title, comment.  
4. **`order_payments.csv`**  
   - Payment method, number of installments, total payment, etc.  
5. **`geolocation.csv`**  
   - Latitude/longitude data for mapping states and regions.  
6. **`sellers.csv`** (less frequently used for the final slides)  
   - Seller IDs, origin addresses, shipping time.

These files provide a **multidimensional** view of Olist’s operations: from **product listings** and **customer satisfaction** to **payment** and **logistics**.

---

## Data Cleaning & Preprocessing
### Null & Missing Values
- **Row Removal**  
  - **Critical Nulls**: Rows in `orders.csv` or `products.csv` with missing essential fields (e.g., price, freight cost, category) were dropped. For instance, ~2,980 rows from `orders.csv` and ~611 from `products.csv`.  
  - **Non-Critical Nulls**: In `order_reviews.csv`, missing review titles/comments didn’t invalidate entire rows since rating data was present. Such rows were kept.

### Category Renaming
- Standardized naming:  
  - Example: `sports_leisure` → **Sports & Leisure**, `bed_bath_table` → **Bed & Bath Table**.  
  - Ensures clarity for plotting and easier interpretation.

### Outlier Detection
- **Freight Cost**: 
  - Boxplots and **IQR** methods identified extremely high freight values (e.g., 1795, 1002).  
  - Removed these outliers (~2–3 rows), preventing skewed comparisons or inflated averages.

### Derived Columns
- **Freight-to-Price Ratio** = `freight_value / price`. Normalizes shipping costs for different price points.  
- **Product Images** = `product_photos_qty`.  
- **Title/Description Length** = e.g., `product_name_length`, `product_description_length`.

**Libraries** used for data cleaning:  

import pandas as pd
import numpy as np
from scipy import stats

---

## Key Insights & Derived Files

### Freight-to-Price Ratio CSV
- Created by merging **`orders.csv`** and **`products.csv`**, then adding `freight_value / price` as a new column.

### Image vs. Orders Analysis
- A separate script/notebook tested **polynomial regression** or **ANOVA** → found p < 0.05 for images correlation.

### Seasonality & Time Series
- Aggregated monthly orders from `order_purchase_timestamp`.  
- Merged with product categories to identify monthly/seasonal peaks.

### Niche Categories
- Subset queries in `order_items` data, grouping by category to track repeat purchase rates.

---

## Technical Details & Libraries

- **Data Wrangling**  
  - `pandas`, `numpy`, `pyjanitor` (optional) for cleaning, merging, group-bys.

- **Visualization**  
  - `matplotlib` or `seaborn` for charts (boxplots, scatter, bar charts).  
  - **Tableau** (.tableau files) for interactive dashboards, geographic maps.

- **Statistical Testing**  
  - `scipy.stats` (t-tests, Shapiro–Wilk, p-values).  
  - `statsmodels` (optional) for regression analysis.

---
---
