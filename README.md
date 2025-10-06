# EDA in Hospitality Domain

![Project: EDA in Hospitality Domain](https://img.shields.io/badge/EDA-Hospitality-blue)

## 📋 Project Overview

This repository performs Exploratory Data Analysis (EDA) on hotel booking data to uncover insights related to **occupancy rates**, **booking trends**, and **revenue performance** across different room categories and cities. The analysis is implemented in a Jupyter Notebook using Python and common data science libraries.

---

## 📂 Dataset

* **`fact_bookings.csv`** — primary dataset containing bookings information such as Booking ID, City, Room Category, Check-in/Check-out dates, Booking Amount/Revenue, and related attributes.

> Place `fact_bookings.csv` in the project root (or update the path in the notebook) before running the notebook.

---

## ⚙️ What this README provides

This file includes:

* A quick **installation** & **run** guide
* A **sample EDA script** (Python) that you can paste into the notebook to reproduce key steps
* Examples for **occupancy**, **trend**, and **revenue** calculations
* Sample code to build common visualizations

---

## 🔧 Requirements

Create a virtual environment and install dependencies:

```bash
python -m venv .venv
source .venv/bin/activate   # macOS / Linux
.venv\Scripts\activate     # Windows PowerShell
pip install -r requirements.txt
```

**requirements.txt** (suggested):

```
pandas
numpy
matplotlib
seaborn
jupyterlab
```

---

## 🚀 How to run

1. Clone the repo:

```bash
git clone https://github.com/shrutisaloni/EDA-in-Hospitality-Domain.git
cd EDA-in-Hospitality-Domain
```

2. Install dependencies (see above)
3. Start Jupyter and open the notebook:

```bash
jupyter notebook
# or jupyter lab
```

4. Open and run the `EDA_in_Hospitality_Domain.ipynb` (or create a new notebook and paste the example code below).

---

## 🧪 Example EDA script (paste into a notebook cell)

```python
# Basic imports
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Load dataset
df = pd.read_csv('fact_bookings.csv', parse_dates=['checkin_date','checkout_date'], dayfirst=False)

# Quick look
print(df.shape)
df.head()

# Data quality checks
print('Missing values:')
print(df.isnull().sum())
print('\nData types:')
print(df.dtypes)

# Basic summary
print(df.describe(include='all'))

# Example: ensure revenue column exists and numeric
if 'booking_amount' in df.columns:
    df['booking_amount'] = pd.to_numeric(df['booking_amount'], errors='coerce')

# Convert checkin_date to month for trend analysis
if 'checkin_date' in df.columns:
    df['month'] = df['checkin_date'].dt.to_period('M').astype(str)

# Occupancy rate calculation by room category (example approach)
# If dataset has 'rooms_booked' and 'rooms_available' or we estimate occupancy from bookings count
if 'room_category' in df.columns:
    occupancy = df.groupby('room_category').size().reset_index(name='bookings')
    occupancy['occupancy_rate_pct'] = (occupancy['bookings'] / occupancy['bookings'].sum()) * 100
    print(occupancy.sort_values('bookings', ascending=False))

# Trend analysis (monthly bookings)
if 'month' in df.columns:
    monthly = df.groupby('month').size().reset_index(name='bookings')
    monthly['month'] = pd.to_datetime(monthly['month'])
    monthly = monthly.sort_values('month')

# Revenue analysis by room category
if 'booking_amount' in df.columns and 'room_category' in df.columns:
    revenue_by_category = df.groupby('room_category')['booking_amount'].agg(['sum','mean','count']).reset_index()
    revenue_by_category = revenue_by_category.sort_values('sum', ascending=False)
    print(revenue_by_category)

# Basic visualizations examples
plt.figure(figsize=(10,5))
sns.barplot(data=occupancy.sort_values('bookings', ascending=False), x='room_category', y='bookings')
plt.title('Bookings by Room Category')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

if 'month' in df.columns:
    plt.figure(figsize=(12,5))
    sns.lineplot(data=monthly, x='month', y='bookings', marker='o')
    plt.title('Monthly Bookings Trend')
    plt.xlabel('Month')
    plt.ylabel('Number of Bookings')
    plt.xticks(rotation=45)
    plt.tight_layout()
    plt.show()

if 'booking_amount' in df.columns and 'room_category' in df.columns:
    plt.figure(figsize=(10,5))
    sns.barplot(data=revenue_by_category, x='room_category', y='sum')
    plt.title('Total Revenue by Room Category')
    plt.xticks(rotation=45)
    plt.tight_layout()
    plt.show()
```

---

## ✅ Suggested next steps / improvements

* Add tests for data quality (expected columns, date ranges)
* Compute true occupancy by merging with inventory data (rooms available per date) if available
* Build interactive dashboards using **Plotly Dash** or **Streamlit**
* Add forecasting (ARIMA / Prophet) for demand prediction
* Create an automated pipeline to refresh EDA with new data

---

## 📌 License

This repository is open for modification and reuse. Add a license (e.g., MIT) if you want to clarify terms.

---

## 🤝 Contributions

Contributions are welcome. Please fork the repo, create a feature branch, and open a pull request with a clear description of changes.

---

## 📫 Contact

If you have questions or want collaboration, open an issue or contact the repo owner.
