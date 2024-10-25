# Product Sales Analysis Project

Welcome to the **Pen and Pencils Product Sales Analysis** repository! This project provides a structured approach to data validation, cleaning, and exploratory analysis for a sales dataset, with a focus on customer engagement through different sales methods. The analysis and recommendations derived here aim to optimize revenue generation strategies for future business decisions.

## Project Overview

This repository presents a comprehensive data analysis pipeline for sales data, focusing on data quality, engagement metrics, and revenue optimization. The project is divided into four main sections:

1. **Data Validation and Cleaning**
2. **Exploratory Data Analysis**
3. **Key Metrics Definition**
4. **Strategic Recommendations**

## Files in this Repository

- **DataCamp.ipynb**: The primary notebook containing the code for data cleaning, exploratory data analysis, and visualization.
- **report.pdf**: A detailed analysis report with recommendations based on the findings from the dataset.

## Data Processing Workflow

### 1. Data Validation and Cleaning

The dataset underwent rigorous cleaning and validation to ensure data integrity.

#### Key Steps

- **Handling Missing Values**: Approximately 7.16% of revenue data was missing. We used mean imputation to fill these gaps:
  ```python
  data['revenue'] = data['revenue'].fillna(data['revenue'].mean())
  ```
- **Removing Duplicates**: Duplicates were identified to prevent redundancy in the dataset:
  ```python
  duplicate_count = data.duplicated().sum()
  print(f"Duplicate entries: {duplicate_count}")
  ```
- **Standardizing Sales Method Labels**: We ensured consistency in the `sales_method` column:
  ```python
  data['sales_method'] = data['sales_method'].replace({'em + call': 'Email + Call'})
  ```
- **Anomaly Detection**: Key columns, such as `week`, `nb_sold`, `revenue`, and `nb_site_visits`, were checked for illogical values, and invalid rows in `years_as_customer` were filtered out:
  ```python
  invalid_years_mask = (data['years_as_customer'] < 0) | (data['years_as_customer'] > (current_year - 1984))
  data_cleaned = data[~invalid_years_mask].reset_index(drop=True)
  ```
  
The cleaned data was saved as `clean_product_sales.csv` for further analysis.

### 2. Exploratory Data Analysis (EDA)

The exploratory analysis provides insights into customer behavior and revenue trends by sales method.

#### Key Insights

- **Customer Count by Sales Method**:
  ```python
  customer_count = data_cleaned['sales_method'].value_counts()
  print(customer_count)
  ```
- **Revenue Distribution**: Revenue data revealed outliers with the most common revenue around 100, showing a right-skewed distribution.
  ```python
  data_cleaned['revenue'].plot(kind='hist', bins=30, title="Revenue Distribution")
  plt.show()
  ```
- **Revenue by Sales Method**:
  ```python
  data_cleaned.groupby('sales_method')['revenue'].median()
  ```
- **Time-Based Revenue Trends**: Weekly revenue patterns showed that `Email + Call` eventually surpassed `Email` in total revenue.
  ```python
  weekly_revenue = data_cleaned.groupby(['week', 'sales_method'])['revenue'].sum().unstack()
  weekly_revenue.plot(title="Weekly Revenue by Sales Method")
  plt.show()
  ```

### 3. Key Metrics

The following metrics were defined to monitor and assess sales performance:

- **Average Revenue per Customer**:
  ```python
  avg_revenue_per_customer = data_cleaned['revenue'].mean()
  print(f"Average Revenue per Customer: {avg_revenue_per_customer}")
  ```
- **Revenue per Sales Method**:
  ```python
  revenue_per_method = data_cleaned.groupby('sales_method')['revenue'].sum()
  print(revenue_per_method)
  ```
- **Average Site Visits per Customer**:
  ```python
  avg_site_visits = data_cleaned['nb_site_visits'].mean()
  print(f"Average Site Visits per Customer: {avg_site_visits}")
  ```

### 4. Recommendations

Based on the findings, the following strategies are recommended:

1. **Prioritize Email**: Leverage Email for broad outreach and automate campaigns.
2. **Optimize Call Strategy**: Focus calls on high-value leads to improve efficiency.
3. **Pilot Email + Call**: Test the Email + Call hybrid method on high-potential leads to validate its impact.
4. **Monitor Key Metrics**: Regular tracking of defined metrics to adapt strategies in real-time.

## Conclusion

The analysis reveals the effectiveness of different sales methods, with `Email` proving efficient for broad reach and `Email + Call` showing high revenue potential. These insights help in resource allocation and strategic planning, providing a data-driven approach to optimize customer engagement and revenue.

## Getting Started

To get started, download or clone this repository and open the `DataCamp.ipynb` notebook to explore the data analysis and findings. Make sure to review `report.pdf` for a more detailed summary and actionable recommendations.
