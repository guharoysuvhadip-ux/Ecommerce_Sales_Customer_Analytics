# E-Commerce Sales & Customer Analytics

An end-to-end data analysis project on a large e-commerce orders dataset: data cleaning, preprocessing, outlier handling, exploratory data analysis (EDA), and machine-learning models that predict **gross sales** and **profit**.

Built in Python (Google Colab) with pandas, NumPy, Matplotlib, Seaborn and scikit-learn.

---

## Project Overview

Online retailers collect a lot of order, customer, delivery and marketing data, but raw data is rarely ready to use. This project takes a raw e-commerce orders file and turns it into insight:

1. Clean the data (missing values, duplicates, sorting).
2. Prepare it for analysis (date conversion, encoding categorical columns).
3. Remove extreme outliers that distort sales figures.
4. Explore patterns in sales, profit, quantity, channels and time.
5. Train regression models to predict `gross_sales` and `profit`.

### Project Objectives

- Understand what drives sales and profit per order.
- Compare sales across years, order quantities and sales channels.
- Build a baseline predictive model and measure how well it performs.

---

## Dataset

| Item | Detail |
|---|---|
| File | `ecommerce_sales_customer_analytics_150k.csv` |
| Rows loaded | 138,116 orders |
| Columns | 46 |
| Period | Orders from 2021 to 2025 |

**Column groups**

- **Order:** `order_id`, `order_date`, `order_time`, `order_status`, `sales_channel`
- **Customer:** `customer_id`, `customer_name`, `customer_age`, `gender`, `customer_segment`, `customer_type`, `is_repeat_customer`, `customer_order_count`, `customer_lifetime_value`
- **Location:** `customer_city`, `customer_state`, `customer_country`, `region`, `customer_postal_code`
- **Payment & shipping:** `payment_method`, `payment_status`, `currency`, `shipping_method`, `warehouse`, `delivery_days`, `estimated_delivery_days`, `delivery_status`
- **Returns & reviews:** `return_status`, `return_reason`, `customer_rating`, `review_sentiment`, `customer_review`
- **Marketing & loyalty:** `marketing_channel`, `campaign_name`, `coupon_code`, `loyalty_points_earned`, `loyalty_points_redeemed`
- **Financials:** `quantity`, `gross_sales`, `discount_amount`, `tax_amount`, `shipping_cost`, `net_sales`, `product_cost`, `profit`, `profit_margin_percentage`

**Missing values:** delivery, rating and review columns have about 24.5k blanks (orders not yet delivered or never reviewed). `return_status` and `return_reason` are blank for orders that were not returned. `campaign_name` and `coupon_code` are blank when no campaign or coupon was used. These blanks are meaningful, not errors.

---

## Workflow

### 1. Data Cleaning
- Inspected shape, data types, summary statistics and null counts.
- Checked for duplicate rows (none found) and sorted by `order_id`.
- Saved the result as `cleaned_data.csv`.

### 2. Preprocessing
- Converted `order_date` to datetime.
- Converted `is_repeat_customer` from True/False to 1/0.
- Label-encoded all text columns (missing values filled with `"Unknown"`).
- Saved the result as `encoded_data.csv`.

### 3. Outlier Removal
- Box plots used to spot extreme values.
- Applied the IQR rule (1.5 × IQR) to `gross_sales` and `quantity`.
- Rows: **138,116 → 129,165** (8,951 removed).
- Saved the result as `outlier_removed_data.csv`.

### 4. Exploratory Data Analysis
- Distribution of gross sales (histogram with KDE).
- Quantity vs gross sales (scatter).
- Product cost vs profit, and profit margin % vs profit (scatter).
- Average gross sales by quantity and by year.
- Share of gross sales by sales channel.
- Correlation heatmap of numeric features.

### 5. Modeling

| | Model 1 | Model 2 |
|---|---|---|
| Target | `gross_sales` | `profit` |
| Algorithm | Linear Regression | Linear Regression |
| Features | `customer_age`, `quantity`, `delivery_days`, `estimated_delivery_days`, `loyalty_points_earned`, `loyalty_points_redeemed` | same |
| Missing values | Mean imputation | Median imputation |
| Split | 80% train / 20% test (`random_state=42`) | same |

Both models use a scikit-learn `Pipeline` (imputer, then regressor). The profit model's coefficients are plotted to show which features move profit the most.

---

## Results

| Metric (gross sales model) | Value |
|---|---|
| R² score | **0.69** |
| Mean absolute error | **≈ 337.62** |

The model explains about 69% of the variation in gross sales on unseen data. An actual-vs-predicted plot is included in the notebook.

---

## Limitations & Next Steps

- **Label encoding** is applied to every text column, including IDs and names, which carry no numeric meaning. One-hot encoding for real categories (channel, segment, region) and dropping ID/name columns would be cleaner.
- **Linear Regression** is only a baseline. Try Random Forest, Gradient Boosting or XGBoost.
- **Missing values:** imputing `delivery_days` with the mean is a simplification. Many blanks mean "not delivered".
- The profit model is trained but its R² and MAE are not reported yet. Add them for a fair comparison.
- The profit-vs-margin scatter is labelled "Price" on the y-axis but plots profit. Fix the label.
- Possible extensions: customer segmentation (RFM / K-Means), return-rate analysis, campaign and coupon effectiveness, time-series sales forecasting.

---

## How to Run

1. Open the notebook in Google Colab, or locally with Jupyter.
2. Upload `ecommerce_sales_customer_analytics_150k.csv` when prompted (or update the file path).
3. Run all cells in order.

**Requirements**

```bash
pip install pandas numpy matplotlib seaborn scikit-learn
```

Note: the notebook uses `google.colab.files.upload()`. Remove it and read the CSV directly if you run locally.

---

## Repository Structure

```
├── ecommerce_sales_customer_analytics_150k.ipynb   # main notebook
├── ecommerce_sales_customer_analytics_150k.csv     # raw data (not included if large)
└── README.md
```

## Tech Stack

Python · pandas · NumPy · Matplotlib · Seaborn · scikit-learn · Google Colab
