# operation_olist, a joint data-visualisation project
---
datasets from: https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce


A **comprehensive exploration** of **Olist**’s e-commerce business in Brazil, covering topics such as **sales drivers**, **repeat purchases**, and **online operational factors** (e.g., listing optimization, payment options).

---

## Table of Contents
1. [Project Context](#project-context)  
2. [Data Sources & Overview](#data-sources--overview)  
3. [Data Cleaning & Preprocessing](#data-cleaning--preprocessing)  
4. [Slide-by-Slide Analysis](#slide-by-slide-analysis)  
   - [1. Brazil E-Commerce Context](#1-brazil-e-commerce-context)  
   - [2. Data Cleaning Details](#2-data-cleaning-details)  
   - [3. Operations & Sales](#3-operations--sales)  
   - [4. Repeat Purchases](#4-repeat-purchases)  
   - [5. Online Operations](#5-online-operations)  
   - [6. Conclusion & Recommendations](#6-conclusion--recommendations)  
5. [Key Insights & Derived Files](#key-insights--derived-files)  
6. [Technical Details & Libraries](#technical-details--libraries)  
7. [Project Structure](#project-structure)  
8. [Acknowledgments & Disclaimer](#acknowledgments--disclaimer)

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

# Slide-by-Slide Analysis

## 1. Brazil E-Commerce Context
**Slides:** “Context: The Brazil E-Commerce Market”, “However… The Vast Geography”, “Growth in Olist’s Business”

- By 2018, Brazil reached **~$18.9 billion** in e-commerce revenue, leading Latin America.  
- **Olist’s 66%** surge from 2016–18 aligns with national growth but underscores **logistical concerns** (long distances, higher costs).  
- Data sources like **PagBrasil** and **EOS Intelligence** highlight these challenges in shipping reliability and cost.

---

## 2. Data Cleaning Details
**Slides:** “Data Cleaning”, “Row Removal”, “Category Renaming”, “Outlier Detection”

- Each file had nulls; we **selectively removed critical rows** (e.g., missing price, freight) but retained partial nulls where feasible.  
- **Freight cost outliers**: Removed after boxplot analysis, ensuring more robust freight comparisons.  
- This **preprocessing** step is vital to achieve **accurate** and **clean** analyses downstream.

---

## 3. Operations & Sales

### Top Product Categories
**Slide:** “Top Selling Products in Brazil”  
- **Result**: ~40% of all products sold fell into:
  1. **Bed & Bath Table**  
  2. **Health & Beauty**  
  3. **Sports & Leisure**  
  4. **Furniture & Decor**  
  5. **Computer Accessories**  

### Geographic Breakdown
**Slide:** “Top Selling Regions in Brazil”  
- **Result**: **São Paulo (SP), Rio de Janeiro (RJ), and Minas Gerais (MG)** account for ~66.7% of total orders.  
- Larger, more urban states → higher purchasing power, thus higher sales.

### Category Combinations
**Slide:** “Is Buying Multiple Categories Together a Factor?”  
- Example combos: **Bed & Bath Table + Furniture Decor**.  
- Indicates that **bundling** might boost average cart value.

### Seasonality
**Slide:** “Is Seasonality a Factor…?”  
- **Nov–Dec** peaks for Toys, Furniture & Decor, etc. → **holiday & Black Friday** effect.  
- **Jan–Feb** spikes for Stationery & Computer Accessories → **back-to-school**.  
- Targeting promotions around these windows can **increase sales**.

### Freight Costs
**Slide:** “Are Shipping and Freight Costs a Factor…?”  
- While shipping cost alone isn’t the biggest determinant of sales volume, **high freight cost** can still dissuade certain buyers (especially if alternatives exist).

---

## 4. Repeat Purchases

### Delivery Reliability
**Slide:** “Does Faster Delivery Times Lead to Higher Chance…”  
- **On-time deliveries** yield the best repeat rate (~5.67%).  
- Early or late arrivals introduce uncertainty.

### Standard Deviation in Delivery Days
**Slide:** “What are the Possible Reasons…?”  
- Late deliveries can range from 1 day to 2+ weeks (std ~15).  
- **Consistency** (predictable timing) seems more important than sheer speed.

### Category Loyalty
**Slide:** “Which Product Categories Have Highest Repeat Purchases?”  
- **La Cuisine** ~15% repeat purchase rate, followed by **Arts & Crafts** and **Fashion: Children**.  
- Suggests smaller, niche categories can build **loyal customer bases**.

### Freight Cost Influence
**Slides:** “Does Higher Freight Cost Impact Repeat Purchasers?”  
- Boxplots reveal that **one-time** customers pay slightly higher freight on average.  
- Lower freight cost correlates with higher loyalty, especially in states near Olist’s HQ (Paraná).

### Geographic Factors
**Slide:** “Do Geographic Factors Impact Repeat Purchases?”  
- Heatmap indicates **southern Brazil** sees higher repeat rates; presumably due to lower shipping costs and on-time deliveries.

---

## 5. Online Operations

### Listing Optimization
**Slides:** “Does the Number of Product Images Affect Sales?”, “Does the Length of Product Title/Description Affect Sales?”  
- ~**6–7 images** yield a **significant** p-value (<0.05) correlation with higher orders per listing.  
- Title/Description length tests had p ~0.14 → **not significant**.

### Payment Options
**Slides:** “Does Payment Options Affect the Number of Orders?”, “Number of Payment Methods”  
- ~**78.34%** of orders use **credit card**; **94.9%** of customers used only **1 method** per order.  
- Payment diversity (multiple methods) is rare.

### Installments
**Slides:** “Does the Presence of Installment Payment Options Influence Total Sales?”  
- ~**71%** of total sales volume use installments, with a **~70.2% higher** average order value.  
- The **number** of installment months doesn’t strongly affect order value, but **having** the option matters.

---

## 6. Conclusion & Recommendations
**Slides:** “Final Recommendations”, “Conclusion”

- **Targeted Advertising**  
  Focus on top regions (SP, RJ, MG). Align ads with product categories that sell best there.

- **Seasonal Promotions**  
  Maximize holiday and back-to-school windows for relevant categories.

- **Bundled Discounts**  
  Combine complementary categories (Bed & Bath Table + Furniture Decor, Housewares + Home Comfort).

- **Logistics & Delivery**  
  Strive for **on-time** over early. Uncertainty hurts loyalty.  
  Consider **satellite hubs** to reduce freight in remote states.

- **Payment Flexibility**  
  Partner with local BNPL (Buy Now, Pay Later) providers (Cleo, ADDI, DiniePay).  
  Encourage installment usage to boost average order value.

- **Listing Requirements**  
  Minimum 3 images, recommended 6–7 to increase listing attractiveness.

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
