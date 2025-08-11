# 📊 Zepto Product Data Analysis – SQL Project

## 📌 Overview

This project focuses on **analyzing Zepto's product inventory dataset** using SQL to generate valuable business insights.
It covers **data cleaning, transformation, exploratory data analysis (EDA), and KPI calculations** for product performance and stock management.

---

## 📂 Dataset

* **Source:** `zepto_v1.csv`
* **Schema:**

  * `sku_id` – Unique product ID
  * `category` – Product category
  * `name` – Product name
  * `mrp` – Maximum Retail Price
  * `discountPercent` – Discount percentage
  * `availableQuantity` – Available stock
  * `discountedSellingPrice` – Selling price after discount
  * `weightInGms` – Product weight in grams
  * `outOfStock` – Boolean (in stock / out of stock)
  * `quentity` – Ordered quantity

---

## 🛠 SQL Operations Performed
**Create Table**
```sql
CREATE TABLE zepto (
    sku_id SERIAL PRIMARY KEY,
    category VARCHAR(120),
    name VARCHAR(120),
    mrp NUMERIC(8,2),
    discountPercent NUMERIC(5,2),
    availableQuantity INTEGER,
    discountedSellingPrice NUMERIC(8,2),
    weightInGms INTEGER,
    outOfStock BOOLEAN,
    quentity INTEGER
);
```
### **1️⃣ Data Exploration**

* Count total rows and view sample data.
```sql
  SELECT COUNT(*) FROM zepto;
```
* Identify null values in important columns.
```sql
  SELECT * FROM zepto
WHERE name IS NULL
   OR category IS NULL
   OR mrp IS NULL
   OR discountPercent IS NULL
   OR discountedSellingPrice IS NULL
   OR weightInGms IS NULL
   OR availableQuantity IS NULL
   OR outOfStock IS NULL
   OR quentity IS NULL;
```
* List unique product categories.
```sql
SELECT DISTINCT category
FROM zepto
ORDER BY category;
```
* Stock status analysis (**in stock vs out of stock**).
```sql
SELECT outOfStock, COUNT(sku_id) AS total_products
FROM zepto
GROUP BY outOfStock;
```
### **2️⃣ Data Cleaning**
* Remove products with zero price.
```sql
SELECT * FROM zepto
WHERE mrp = 0 OR discountedSellingPrice = 0;

DELETE FROM zepto
WHERE mrp = 0;
```
* Convert prices from paise to rupees.
```sql
UPDATE zepto
SET mrp = mrp / 100.0,
    discountedSellingPrice = discountedSellingPrice / 100.0;
```
### **3️⃣ Analysis & Insights**

* **Top 10 best-value products** based on discount percentage.
  
```sql
SELECT DISTINCT name, mrp, discountPercent
FROM zepto
ORDER BY discountPercent DESC
LIMIT 10;
```
* **High MRP but out of stock** products.
```sql
SELECT DISTINCT name, mrp
FROM zepto
WHERE outOfStock = TRUE AND mrp > 300
ORDER BY mrp DESC;
```
* **Calculate estimated revenue for each category**.
```sql
SELECT category,
       SUM(discountedSellingPrice * availableQuantity) AS total_revenue
FROM zepto
GROUP BY category
ORDER BY total_revenue DESC;
```
* Products with **high MRP but low discount**.
```sql
  SELECT DISTINCT name, mrp, discountPercent
FROM zepto
WHERE mrp > 500 AND discountPercent < 10
ORDER BY mrp DESC, discountPercent DESC;
```
* **Identify top 5 categories** offering the highest average discount percentage.
```sql
SELECT category,
       ROUND(AVG(discountPercent), 2) AS avg_discount
FROM zepto
GROUP BY category
ORDER BY avg_discount DESC
LIMIT 5;
``` 
* **Price per gram** for products above 100g.
```sql
SELECT DISTINCT name, weightInGms, discountedSellingPrice,
       ROUND(discountedSellingPrice / weightInGms, 2) AS price_per_gram
FROM zepto
WHERE weightInGms >= 100
ORDER BY price_per_gram ASC;
```
* Classifying products into **Low, Medium, Bulk** weight categories.
```sql
SELECT DISTINCT name, weightInGms,
       CASE 
           WHEN weightInGms < 1000 THEN 'Low'
           WHEN weightInGms < 5000 THEN 'Medium'
           ELSE 'Bulk'
       END AS weight_category
FROM zepto;
```
* **Total inventory weight** per category.
```sql
SELECT category, 
       SUM(weightInGms * availableQuantity) AS total_weight
FROM zepto
GROUP BY category
ORDER BY total_weight DESC;
```
---

## 💡 Key Skills Used

* **SQL:** SELECT, WHERE, GROUP BY, ORDER BY, HAVING
* **Data Cleaning:** Filtering, updating, deleting
* **Aggregations:** SUM, AVG, COUNT, ROUND
* **Logic:** CASE statements for categorization
* **Insight Generation:** Pricing trends, stock analysis, category performance

---

## 🚀 How to Run

1. Import `zepto_v1.csv` into your database (PostgreSQL/MySQL).
2. Run the SQL script `zepto_sql_project.sql`.
3. Analyze the results to generate insights.

---

## 📌 Outcomes

* Identified high-value discount products for promotions.
* Revealed underperforming categories and low-stock high-MRP items.
* Provided revenue-based category ranking for better inventory planning.

---

## 👤 Author

**Satyam Pal**
📧 Email: [12345satyampal@gmail.com](mailto:12345satyampal@gmail.com)
📞 Phone: +91 7501643011
🔗 [LinkedIn](https://www.linkedin.com/in/satyam-pal-606404314/) | [GitHub](https://github.com/satyampal123-sp)

---
