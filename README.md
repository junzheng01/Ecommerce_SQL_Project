# E-Commerce Data Analysis Project

This project focuses on creating a database for an e-commerce platform, performing data cleaning, and conducting detailed data analysis to gain insights into customer demographics, product performance, and review analysis. Below is a breakdown of the project structure and the SQL queries used for each step.

## Table of Contents
1. [Database Creation](#database-creation)
2. [Table Creation](#table-creation)
3. [Data Cleaning](#data-cleaning)
4. [Data Analysis](#data-analysis)
   - [Customer Demographics](#customer-demographics)
   - [Product Performance](#product-performance)
   - [Review Analysis](#review-analysis)

## Database Creation
The first step is to create a database named `Project_ECOMM` to store all the e-commerce data.

```sql
CREATE DATABASE Project_ECOMM;
```

## Table Creation
Next, we create a table named ECommerce to store the e-commerce data. The table includes columns for product details, customer demographics, and review information.

```sql

DROP TABLE IF EXISTS Ecommerce;

CREATE TABLE ECommerce (
    Product_ID Text PRIMARY KEY,
    Product_Name Varchar(20),
    Category Varchar(15),
    Sub_Category Varchar(15),
    Price FLOAT,
    Customer_Age INT,
    Customer_Gender Varchar(15),
    Purchase_History INT,
    Review_Rating INT,
    Review_Sentiment Varchar(20)
);
```
#Data Cleaning
Before performing any analysis, it's crucial to clean the data by removing any rows with missing values.

```sql
-- Check for NULL values
SELECT * 
FROM Ecommerce
WHERE product_id IS NULL 
    OR product_name IS NULL
    OR category IS NULL
    OR sub_category IS NULL 
    OR price IS NULL 
    OR customer_age IS NULL 
    OR customer_gender IS NULL 
    OR purchase_history IS NULL 
    OR review_rating IS NULL 
    OR review_sentiment IS NULL;

-- Delete rows with NULL values
DELETE 
FROM Ecommerce 
WHERE product_id IS NULL 
    OR product_name IS NULL
    OR category IS NULL
    OR sub_category IS NULL 
    OR price IS NULL 
    OR customer_age IS NULL 
    OR customer_gender IS NULL 
    OR purchase_history IS NULL 
    OR review_rating IS NULL 
    OR review_sentiment IS NULL;
```
## Data Analysis
The data analysis is divided into three main sections: Customer Demographics, Product Performance, and Review Analysis.

Customer Demographics
A.1 Distribution of Customer Ages
This query categorizes customers into age groups and calculates the percentage distribution.
```sql
SELECT 
    CASE 
        WHEN Customer_Age BETWEEN 18 AND 25 THEN '18-25'
        WHEN Customer_Age BETWEEN 26 AND 35 THEN '26-35'
        WHEN Customer_Age BETWEEN 36 AND 45 THEN '36-45'
        ELSE '46+'
    END AS Age_Group,
    COUNT(*) AS Customer_Count,
    ROUND(COUNT(*) * 100.0 / SUM(COUNT(*)) OVER (), 2) AS Percents
FROM ECommerce
GROUP BY Age_Group
ORDER BY Age_Group;
```
A.2 Distribution of Customer Genders
This query calculates the distribution of customer genders.
```sql
SELECT Customer_gender, 
    COUNT(customer_gender) AS Total, 
    ROUND(COUNT(Customer_gender)*100 / SUM(COUNT(Customer_gender)) OVER(), 2) AS Percents
FROM Ecommerce 
GROUP BY customer_gender;
```

Product Performance
B.1 Rank Sub-Categories in the Beauty Category by Most Products Sold
This query ranks sub-categories within the Beauty category based on the number of products sold.
```sql
SELECT Sub_category, 
    COUNT(sub_category) AS Product_Count,
    RANK() OVER (ORDER BY COUNT(sub_category) DESC) AS Rnk
FROM Ecommerce
WHERE Category = 'Beauty'
GROUP BY Sub_category;
```
#B.2 Average Price by Category and Sub-Category
This query calculates the average price for each category and sub-category.

```sql
SELECT category, sub_category, AVG(price) AS "Average Price"
FROM Ecommerce
GROUP BY category, sub_category
ORDER BY "Average Price";
```
#B.3 Products with the Highest and Lowest Prices
These queries identify the top 5 highest and lowest priced products.
```sql
-- Highest priced products
SELECT Product_Name, Price
FROM Ecommerce
ORDER BY Price DESC
LIMIT 5;

-- Lowest priced products
SELECT Product_Name, Price
FROM Ecommerce
ORDER BY Price ASC
LIMIT 5;
```
Review Analysis
C.1 Distribution of Review Ratings
This query categorizes reviews by rating and calculates the percentage distribution.
```sql
SELECT CASE
    WHEN review_rating = 1 THEN '1 Star'
    WHEN review_rating = 2 THEN '2 Star'
    WHEN review_rating = 3 THEN '3 Star'
    WHEN review_rating = 4 THEN '4 Star'
    ELSE '5 Star'
    END AS Ratings, 
    COUNT(*) AS Total_Ratings,
    ROUND(COUNT(*) * 100.0 / SUM(COUNT(*)) OVER (), 2) AS Percents
FROM Ecommerce
GROUP BY Ratings
ORDER BY Ratings;
```
C.2 Distribution of Review Sentiment
This query calculates the distribution of review sentiments (positive, neutral, negative).

```sql
SELECT 
    Review_Sentiment, 
    COUNT(*) AS Total_Sentiments,
    ROUND(COUNT(*) * 100.0 / SUM(COUNT(*)) OVER (), 2) AS Percents
FROM Ecommerce
GROUP BY Review_Sentiment
ORDER BY Total_Sentiments DESC;
```
C.3 Review Ratings and Sentiment by Product Category
This query analyzes how review ratings and sentiments vary across different product categories.
```sql
SELECT Category,
    CASE
        WHEN review_rating = 1 THEN '1 Star'
        WHEN review_rating = 2 THEN '2 Star'
        WHEN review_rating = 3 THEN '3 Star'
        WHEN review_rating = 4 THEN '4 Star'
        ELSE '5 Star'
    END AS Ratings, 
    COUNT(*) AS Total_Ratings,
    ROUND(COUNT(*) * 100.0 / SUM(COUNT(*)) OVER (PARTITION BY Category), 2) AS Percents
FROM Ecommerce
GROUP BY Category, Ratings
ORDER BY Category, Ratings;
```
## Conclusion
This project provides a comprehensive analysis of an e-commerce dataset, offering insights into customer demographics, product performance, and review trends. The SQL queries included can be used to replicate the analysis or adapt it to similar datasets.

For any questions or further information, please feel free to reach out.
