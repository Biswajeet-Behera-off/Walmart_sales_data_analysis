# Walmart Sales Data Analysis

## 📊 Project Overview
This project involves setting up a **Walmart sales database** and performing an in-depth analysis to derive insights on sales performance, customer preferences, and product trends. SQL is used extensively for data loading, feature engineering, and analysis.

## 🗂️ Database Setup and Data Loading

### Database Creation and Data Import
1. **Database Creation**: Created a `walmart` database and defined the `sales` table structure for efficient querying.
2. **Data Import**: Loaded sales data from a CSV file into the `sales` table using SQL's `LOAD DATA` command.

```sql
-- Create the database and sales table
CREATE DATABASE IF NOT EXISTS walmart;
USE walmart;

CREATE TABLE IF NOT EXISTS sales (
    invoice_id VARCHAR(30) PRIMARY KEY,
    branch VARCHAR(5),
    city VARCHAR(30),
    customer_type VARCHAR(30),
    gender VARCHAR(10),
    product_line VARCHAR(100),
    unit_price DECIMAL(10, 2),
    quantity INT,
    vat FLOAT(6, 4),
    total DECIMAL(12, 4),
    date DATETIME,
    time TIME,
    payment VARCHAR(15),
    cogs DECIMAL(10, 2),
    gross_margin_pct FLOAT(11, 9),
    gross_income DECIMAL(12, 4),
    rating FLOAT(2, 1)
);
```

## 🧩 Feature Engineering
To enhance the analysis, several new columns were created:

| Feature             | Description                                     |
|---------------------|-------------------------------------------------|
| `time_of_day`       | Categorizes transaction times as Morning, Afternoon, or Evening |
| `day_name`          | Day of the week for each transaction            |
| `month_name`        | Month name for each transaction date            |
| `product_category`  | Labels each product as "Good" or "Bad" based on performance compared to the average |

```sql
-- Example of adding a feature for time of day
ALTER TABLE sales ADD COLUMN time_of_day VARCHAR(20);
UPDATE sales SET time_of_day = 
    CASE WHEN time BETWEEN '00:00:00' AND '12:00:00' THEN 'Morning'
         WHEN time BETWEEN '12:01:00' AND '16:00:00' THEN 'Afternoon'
         ELSE 'Evening'
    END;
```

## 🔍 Exploratory Data Analysis (EDA)

### Key Business Questions and Insights
1. **Branch and Product Analysis**
   - **Top-selling products** and **revenue-generating branches** were identified.
   - **Monthly revenue trends** showed peak sales periods.

2. **Sales Patterns by Time and Location**
   - Sales analysis by **time of day** and **day of the week** helped optimize staffing and inventory for peak periods.
   - **City-level sales** identified top revenue-generating cities, guiding regional marketing strategies.

3. **Customer Demographics and Preferences**
   - Analyzed customer distribution by **gender, customer type**, and payment method to tailor marketing strategies.

### Sample SQL Queries
```sql
-- Find the top-selling product lines
SELECT product_line, COUNT(product_line) AS sales_volume
FROM sales GROUP BY product_line ORDER BY sales_volume DESC;

-- Monthly revenue analysis
SELECT month_name, SUM(total) AS total_revenue
FROM sales GROUP BY month_name ORDER BY total_revenue DESC;
```

### Summary of Key Findings
| Question                                         | SQL Snippet                                                                                              |
|--------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| **Top product lines**                            | `SELECT product_line, COUNT(*) FROM sales GROUP BY product_line ORDER BY COUNT(*) DESC;`                 |
| **Highest average customer rating branch**       | `SELECT branch, AVG(rating) FROM sales GROUP BY branch ORDER BY AVG(rating) DESC LIMIT 1;`               |
| **City with highest VAT contribution**           | `SELECT city, SUM(vat) FROM sales GROUP BY city ORDER BY SUM(vat) DESC LIMIT 1;`                         |
| **Gender distribution by branch**                | `SELECT branch, gender, COUNT(gender) FROM sales GROUP BY branch, gender ORDER BY branch;`               |

## 🛠️ Technologies Used
- **SQL**: For database creation, data transformation, and analysis
- **CSV File Loading**: `LOAD DATA LOCAL INFILE` to import large datasets
- **Microsoft Excel**: Initial data cleaning and formatting

## 📈 Conclusion
This project offers valuable insights into Walmart’s sales patterns, customer behavior, and product performance, providing a basis for data-driven decision-making in retail management.
