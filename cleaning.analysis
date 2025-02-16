--Create Database
CREATE DATABASE Project_ECOMM;

-- Create Table
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

--Data Cleaning
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
	or review_sentiment IS NULL;

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
	or review_sentiment IS NULL;

--Data Analysis 
-- A. Customer Demographics
-- What is the distribution of customer ages? (age groups: 18-25, 26-35, 36-45, 46+.)

-- What is the distribution of customer genders?

-- B. Product Performance
-- Which product categories and sub-categories have the most products?

-- What is the average price by category and sub-category?

-- Which products have the highest and lowest prices?

-- C. Review Analysis
-- What is the distribution of review ratings (e.g., 1-5 stars)?

-- What is the distribution of review sentiment (e.g., positive, neutral, negative)?

-- How do review ratings and sentiment vary by product category?

--A.1 What is the distribution of customer ages? (age groups: 18-25, 26-35, 36-45, 46+.)
SELECT MIN(customer_age) FROM Ecommerce;

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

--A.2 What is the distribution of customer genders?
SELECT Customer_gender, 
COUNT(customer_gender) AS Total, 
ROUND(COUNT(Customer_gender)*100 / SUM(COUNT(Customer_gender)) OVER(), 2) AS Percents
FROM Ecommerce 
GROUP BY customer_gender;

-- B. Product Performance
-- B.1 Rank the sub-categories in the Beauty Category by most products sold.

SELECT Sub_category, 
COUNT(sub_category) AS Product_Count,
RANK() OVER (ORDER BY COUNT(sub_category) DESC) AS Rnk
FROM Ecommerce
WHERE Category = 'Beauty'
GROUP BY Sub_category;

-- B.2 What is the average price by category and sub-category?
SELECT category, sub_category, AVG(price) AS "Average Price"
FROM Ecommerce
GROUP BY category, sub_category
ORDER BY "Average Price"; 

-- B.3 Which products have the highest and lowest prices?
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

-- C. Review Analysis
-- C.1 What is the distribution of review ratings (e.g., 1-5 stars)?

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

-- C.2 What is the distribution of review sentiment (e.g., positive, neutral, negative)?
SELECT 
Review_Sentiment, 
COUNT(*) AS Total_Sentiments,
ROUND(COUNT(*) * 100.0 / SUM(COUNT(*)) OVER (), 2) AS Percents
FROM Ecommerce
GROUP BY Review_Sentiment
ORDER BY Total_Sentiments DESC;

-- C.3 How do review ratings and sentiment vary by product category
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

