# Customerloyaltyprogram Data Analysis using SQL

## Overview

This project consists a detailed analysis of CUSTOMERLOYALTYPROGRAM using SQL. The goal of the project is to extract valuable insight and answer various business questions based on the dataset with the POWER BI visualization dashboard

The following README provide  a comprehensive account of the project’s objective, business problem, finding and conclusion.

## Objective
General Question/Business Question

- A Which country generate highest profits
--Q1.Identify the country that contributes the most profit to the customerloyaltyprogram

- B What are top 5 country with highest profit
-- Q.2 Which country generate the most profits for the business

- C. What are the top 5 country that has highest total cost
-- Q.3 Which country incur the highest costs and how can cast be optimize in this regions

- D. What is the total profit for each product line
-- Q.4 Determine which product lines are most profitable to guide products line strategy

- E. Who are the most customername by product line?
--  Q.5 identify the most product line to focused on retaining & rewarding

- F. What is the average profit per sale by country
-- Q.6 How average profit varies by country to assess market efficiency

- G. What are total revenue for each customer
-- Q.7 How much revenue does each customer generate

## Dataset
The data for this project is from Kaggle.com dataset:

Datalink [https://www.kaggle.com/datasets/ranja7/customerloyaltytier-trainset]

## Schema

-- CustomerLoyaltyProgram Project

DROP TABLE IF EXISTS customerloyaltyprogram;

CREATE TABLE customerloyaltyprogram
(
	Loyalty INT,
	First_Name VARCHAR(15),
	Last_Name VARCHAR(15),
	CustomerName VARCHAR(30),
	Country	VARCHAR(15),
	Province_States TEXT,
	City VARCHAR(25),
	Latitude DECIMAL,
	Longitude DECIMAL,
	Postal_code VARCHAR(10),
	Gender VARCHAR(10),
	Education VARCHAR(25),
	Locations_Code TEXT,
	Income INT,
	Marital_Status VARCHAR(10),
	Order_Year INT,
	Quarter	VARCHAR(5),
	MonthsAsMember INT,
	LoyaltyStatus VARCHAR(10),
	Product_Line VARCHAR(25),
	Coupon_Response VARCHAR(10),
	Count INT,
	Quantity_Sold INT,
	Unit_Sale_Price FLOAT,
	Unit_Costs FLOAT,
	Revenue FLOAT,
	Customer_Lifetime_Value FLOAT,
	Loyalty_Count INT
);


-- Data Analysis Report
-- General Business Question And Solutions

-- Q.1 Retrieve all the columns from customerloyaltyprogram

```SELECT * FROM customerloyaltyprogram;```

-- Q.2 Retrieve customername, country revenue where education columns is Bachelor
```SELECT 
	customername,
	country,
	revenue
FROM customerloyaltyprogram
WHERE education = 'Bachelor'```

-- Q.3 Customerloyaltyprogram Count
-- How many people in each customername are estimated to loyaltyprogram by revenue

SELECT 
	SUM(revenue) as total_revenue
FROM customerloyaltyprogram
WHERE country = 'Canada'

--Q.4 Find the count of all customerloyalty as total_content
SELECT COUNT(*) as total_content
FROM customerloyaltyprogram;
	 
-- Q.5 Find the unique column of gender from customerloyalty	 
SELECT DISTINCT gender FROM customerloyaltyprogram;

-- Q.6 Count the number of female vs male as total_content
SELECT gender, 
		COUNT(*) as total_content
FROM customerloyaltyprogram
GROUP BY gender;


--- Q.7 Find the revenue, income, revenue, gender and group 
-- and order in descending order from customerloyalty

SELECT gender,
		income,
		revenue,
		COUNT(*)
FROM customerloyaltyprogram
GROUP BY 1,2,3
ORDER BY 3 DESC

--Q.8 List all female in the gender where income is greater than 50000

SELECT * FROM customerloyaltyprogram
WHERE gender = 'female'AND income > 50000

--Q.9 Find the top countries with most content by customer lifetime values
-- and group by country and loyaltystautus

SELECT country,
		loyaltystatus,
		COUNT (customer_lifetime_value) as total_content
FROM customerloyaltyprogram
GROUP BY 1, 2;

-- Q.10 Find prinvince_state where british columbia, bremen, sudbury & 
-- reading were omitted from customerloyatyprograme

SELECT *
FROM customerloyaltyprogram
WHERE province_states NOT IN('British Columbia','Bremen','Sudbury','Reading');

-- Q.11 Find the minimum price for customerloyalty as lessersprice

SELECT Min(unit_sale_price) as lesserprice
FROM customerloyaltyprogram;

-- Q.12 Find the maximum price for customerloyalty as highsprice

SELECT Max(unit_sale_price) as lesserprice 
FROM customerloyaltyprogram;


-- Q.13 Which of the country & revenue greater than 6000 

SELECT DISTINCT(country) FROM customerloyaltyprogram
WHERE revenue > 6000;

-- Q.14 Find the average cost from customerloyaltyprogram

SELECT AVG(unit_costs) FROM customerloyaltyprogram;


-- Q.15 from table give discount 0f 10% by customername, country, loyaltystatus, revenue

SELECT customername, 
		country, 
		loyaltystatus,
		revenue,
		(revenue + 10) * 100 as discount
FROM customerloyaltyprogram;

--Q.16 Return all customerloyalty, customer name unit price and the province_state  
-- is manitoba

SELECT 
	customername, 
	unit_sale_price
FROM customerloyaltyprogram
WHERE province_states = 'Manitoba'

-- Q.17 Return all customerloyalty, customer name unit price and new price with 
-- 10% increase

SELECT 
	customername, 
	unit_sale_price,
	(unit_sale_price * 1.1) As new_unit_sale_price
FROM customerloyaltyprogram;

--Q.18 Return all customerloyalty, customer name unit price and the province_state  
-- that is not in Manitoba

SELECT 
	customername, 
	unit_sale_price
FROM customerloyaltyprogram
WHERE province_states != 'Manitoba'

--Q.19 Find customerloyalty, customer name unitsale price, revenue 
--the province_state that is not in Manitoba and revenue greater than 2000

SELECT 
	customername, 
	unit_sale_price,
	revenue
FROM customerloyaltyprogram
WHERE province_states != 'Manitoba' AND revenue > 2000

--Q.20 Retrieved all customerloyalty, customer name unitsale price, revenue 
-- and revenue greater than 2000 AND education is master.

SELECT 
	customername, 
	unit_sale_price,
	revenue,
	education
FROM customerloyaltyprogram
WHERE revenue > 2000  AND education = 'Master'


--Q.21 Find customerloyalty, order by country and loyaltystatus
-- and revenue greater than 2000 AND education is Bachelor with not more than limit 10

SELECT *
FROM customerloyaltyprogram
WHERE revenue > 2000  AND education = 'Bachelor'
ORDER BY country DESC, loyaltystatus DESC
LIMIT 10



-- Top Business Question

--A Which country generate higest profits
--Q1.Identify the country that contributes the most profit to the customerloyaltyprograme

SELECT * FROM customerloyaltyprogram;

SELECT country, SUM(revenue * unit_costs) As total_profit
FROM customerloyaltyprogram
GROUP BY country
ORDER BY total_profit DESC
LIMIT 5;

-- B What are top 5 country with highest profit
-- Q.2 Which country generate the most profits for the business
 
SELECT country, SUM(revenue - quantity_sold * unit_costs ) As total_profit
FROM customerloyaltyprogram
GROUP BY country
ORDER BY total_profit DESC
LIMIT 5;

-- C. What are the top 5 country that has higest total cost
-- Q.3 Which country incur the higest costs and how can cast be optimize in this regions

SELECT country, SUM(quantity_sold * unit_costs) As total_cost
FROM customerloyaltyprogram
GROUP BY country
ORDER BY total_cost DESC
LIMIT 5;


-- D. What is the total profit for each product line
-- Q.4 Determine which product lines are most profitable to guide products line strategy

SELECT product_line, SUM(revenue - (quantity_sold * unit_costs)) As total_profit
FROM customerloyaltyprogram
GROUP BY product_line
ORDER BY total_profit DESC

-- E. Who are the most customername by product line?
-- Q.5 identify the most product line to focuse on retaining & rewarding

SElECT customername, product_line
FROM customerloyaltyprogram
where gender ='female'


-- F. What is the average profit per sale by country
-- Q.6 How average profit varies by country to assess market efficency

SELECT country, AVG(revenue - (quantity_sold * unit_costs)) As avg_profit_per_sale
FROM customerloyaltyprogram
GROUP BY country
ORDER BY avg_profit_per_sale DESC

--G. What are total revenue for each customer
--Q.7 How much revenue does each customer generate

SELECT customername, SUM(revenue) As total_revenue
FROM customerloyaltyprogram
GROUP BY customername
ORDER BY total_revenue DESC


These SQL queries are design to address key business metrics such as revenue profits, cost, customer value and product performance
![image](https://github.com/user-attachments/assets/ee814824-1d0a-4877-9406-a72d758d15e4)




