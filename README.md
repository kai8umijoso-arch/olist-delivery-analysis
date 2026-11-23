# Olist Delivery & Review Analysis

## Project overview

This project analyzes the relationship between delivery performance and customer review scores using the **Olist Brazilian E-Commerce Public Dataset**. The dataset is publicly available on Kaggle and is used here as an example of an e-commerce platform; it is not proprietary data from ZOZO.

The analysis distinguishes two different delay definitions:

- **Customer delivery delay:** actual delivery to the customer is later than the estimated delivery date.
- **Seller shipping handoff delay:** the carrier handoff timestamp is later than the item-level shipping deadline.

These indicators describe different stages of the fulfillment process and should not be treated as interchangeable. The analysis uses observational data and does not establish causal relationships.

## Dataset

- **Dataset:** [Brazilian E-Commerce Public Dataset by Olist (Kaggle)](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
- **Publisher:** Olist
- **Source:** Kaggle

Download the dataset from Kaggle and extract the archive. This notebook uses the following four CSV files:

| CSV file | Purpose |
|---|---|
| `olist_orders_dataset.csv` | Order status, purchase date, carrier handoff date, actual delivery date, and estimated delivery date |
| `olist_order_reviews_dataset.csv` | Customer review scores |
| `olist_order_items_dataset.csv` | Order-item details, seller IDs, and item-level shipping deadlines |
| `olist_products_dataset.csv` | Product IDs and product categories |

Other CSV files included in the original Olist dataset are not required by this notebook.

**The dataset files are not included in this repository.** Download them directly from Kaggle and store the four required CSV files in the data folder described below.

## Environment

- **Execution environment:** Google Colab
- **Language:** Python 3.x (the Python version provided by the Colab runtime; it may change over time)
- **Libraries:**
  - `pandas`
  - `numpy`
  - `matplotlib`
  - `seaborn`
  - `scipy`
- **Storage:** Google Drive, mounted from Google Colab

The notebook uses the libraries available in the Colab runtime. Exact package versions are therefore not pinned in this repository and may vary. To check the versions in your current runtime, run:

```python
import sys
import pandas as pd
import numpy as np
import matplotlib
import seaborn as sns
import scipy

print("Python:", sys.version)
print("pandas:", pd.__version__)
print("numpy:", np.__version__)
print("matplotlib:", matplotlib.__version__)
print("seaborn:", sns.__version__)
print("scipy:", scipy.__version__)
```

## How to run in Google Colab

1. Download the [Olist dataset from Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) and extract the ZIP archive.
2. In Google Drive, create the following folder structure:

   ```text
   MyDrive/
   └── ZOZO_DataAnalysis/
       └── data/
           ├── olist_orders_dataset.csv
           ├── olist_order_reviews_dataset.csv
           ├── olist_order_items_dataset.csv
           └── olist_products_dataset.csv
   ```

   Place the four required CSV files in the `data` folder. The folder name is retained as the project's working directory name; it does not mean the dataset is from ZOZO.

3. Open `analysis.ipynb` in [Google Colab](https://colab.research.google.com/). You can upload the notebook to Colab or open it from your GitHub repository.
4. Select **Runtime → Run all**. When prompted, authorize Colab to access your Google Drive. The notebook mounts Drive at `/content/drive`.
5. The notebook reads the four CSV files from:

   ```text
   /content/drive/MyDrive/ZOZO_DataAnalysis/data
   ```

6. Run the cells from top to bottom and review the generated tables and charts.

If you store the CSV files in a different location, update `DATA_DIR` in the data-loading cell of `analysis.ipynb` to the folder containing the four files. If a required file is missing, the notebook displays an error listing the missing filenames.

## Analysis questions

1. Do customer-delayed orders have different review scores or low-review rates from orders delivered by the estimated date?
2. Which part of the delivery process takes longer for delayed orders?
3. Which sellers and product categories have relatively high shipping-deadline overdue rates?
4. Are there seller × category combinations that may deserve further operational investigation?
5. Is seller-level shipping overdue rate associated with seller-level average review score?

## Methods

- Convert timestamp columns to datetime.
- Aggregate multiple review rows to one mean review score per order.
- Analyze customer delivery outcomes at the order level.
- Calculate order-to-carrier and carrier-to-customer durations.
- Determine item-level shipping deadline overdue status by comparing `shipping_limit_date` with `order_delivered_carrier_date`.
- Aggregate seller analysis at order × seller level and seller-category analysis at order × seller × category level to avoid counting the same order review multiple times just because the order contains multiple item rows.
- Use a minimum of 30 records for seller/category comparisons as a practical screening threshold.
- Calculate Pearson and Spearman correlations for seller-level overdue rate and average review score.

## Important interpretation notes

- Seller shipping handoff delay and customer delivery delay are different measures.
- Correlation does not establish causation.
- Review scores may be affected by product quality, price, customer service, and other factors beyond delivery.
- The 30-record threshold is a screening rule, not a guarantee of statistical reliability.
- The notebook excludes records without the timestamps needed for each specific comparison.
- The dataset reflects a particular historical market and period; findings should not be generalized to all current e-commerce businesses without further validation.

## Repository structure

```text
olist-delivery-analysis/
├── analysis.ipynb
└── README.md
```
