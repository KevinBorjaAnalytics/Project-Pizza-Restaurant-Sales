# Project Pizza Restaurant Sales
 SQL & Power Bi Data Analysis

## Table of Contents
- [Project Pizza Restaurant Sales](#project-pizza-restaurant-sales)
  - [Table of Contents](#table-of-contents)
  - [1. Introduction:](#1-introduction)
      - [1.1 Company Background](#11-company-background)
  - [2. PREPARE Phase](#2-prepare-phase)
      - [2.1 Data Used](#21-data-used)
      - [2.2 Accessibility \& Usage of Data](#22-accessibility--usage-of-data)
      - [2.3 Data Limitations \& Integrity](#23-data-limitations--integrity)
    - [2.4 Tools and Methodologies](#24-tools-and-methodologies)
      - [Tools:](#tools)
  - [3. Prepare Phase](#3-prepare-phase)
      - [3.1 Data Preparation](#31-data-preparation)
      - [3.2 Data cleaning](#32-data-cleaning)
      - [3.2 Data Normalization](#32-data-normalization)
      - [3.3 Data Transformation](#33-data-transformation)

## 1. Introduction: 
#### 1.1 Company Background
For the Maven Pizza Challenge, you’ll be playing the role of a BI Consultant hired by Plato's Pizza, a Greek-inspired pizza place in New Jersey. You've been hired to help the restaurant use data to improve operations, and just received the following note:

Welcome aboard, we're glad you're here to help!

Things are going OK here at Plato's, but there's room for improvement. We've been collecting transactional data for the past year, but really haven't been able to put it to good use. Hoping you can analyze the data and put together a report to help us find opportunities to drive more sales and work more efficiently.

Here are some questions that we'd like to be able to answer:

- What days and times do we tend to be busiest?
- How many pizzas are we making during peak periods?
- What are our best and worst-selling pizzas?
- What's our average order value?
- How well are we utilizing our seating capacity? (we have 15 tables and 60 seats)

That's all I can think of for now, but if you have any other ideas I'd love to hear them – you're the expert!
Thanks in advance,

Mario Maven (Manager, Plato's Pizza) 

## 2. PREPARE Phase
#### 2.1 Data Used 
The data source used for this Project is https://www.kaggle.com/datasets/shilongzhuang/pizza-sales. This dataset was downloaded from Kaggle where it was uploaded by Shi Long Zhuang.

#### 2.2 Accessibility & Usage of Data 
This public dataset is completely available on the Maven Analytics website platform where it stores and consolidates all available datasets for analysis in the Data Playground. The specific individual datasets at hand can be obtained at this link below: https://www.mavenanalytics.io/blog/maven-pizza-challenge

#### 2.3 Data Limitations & Integrity
This Pizza Sales dataset contains 12 columns and a total of 48620 records:
- **order_id**: Unique identifier for each order placed by a table.
- **order_details_id**: Unique identifier for each order placed by a table.
- **pizza_id**: Unique key identifier that ties the pizza ordered to its details, like size and price.
- **quantity**: Quantity ordered for each pizza of the same type and size.
- **order_date**: Date the order was placed (entered into the system prior to cooking & serving).
- **order_time**: Time the order was placed (entered into the system prior to cooking & serving).
- **unit_price**: Price of the pizza in USD.
- **total_price**: Unit_price * quantity.
- **pizza_size**: Size of the pizza (Small, Medium, Large, X Large, or XX Large).
- **pizza_type**: Unique key identifier that ties the pizza ordered to its details, like size and price.
- **pizza_ingredients**: Ingredients used in the pizza as shown in the menu (they all include Mozzarella Cheese, even if not specified; and they all include Tomato Sauce, unless another sauce is specified).


The data is clean and well-maintained but somewhat limited:

There is not enough information to conduct an in-depth Inventory Analysis which provides real-time insights into inventory levels, stock movements, and related metrics that help to optimize supply chain operations and minimize costs which can ensure efficient stock levels.

There is also no Customer information for Customer Segmentation Analysis, a process of dividing a company's customers into groups based on shared characteristics to better tailor marketing and sales efforts.

### 2.4 Tools and Methodologies
#### Tools:
- MYSQL Workbench is the tool that will be used to create a database Schema for the Pizza Restaurant Sales dataset.
Here we will take the appropriate cleaning procedures if necessary and normalise the data.
Once the data preparation process is taken care of we will undergo Exploratory Data Analysis (EDA) by querying the data to gain valuable insights.

- POWER BI will be used for visualizations and to build a Dashboard

Data analysis was conducted using Python for statistical computations and Tableau for visualizations. The methodologies included correlation analysis, linear regression, and cluster analysis to identify patterns and draw insights.

## 3. Prepare Phase
#### 3.1 Data Preparation 
We start off by creating a new database called **pizza_db** in MYSQL and import the relevant csv file in which all 48620 records were successfully imported.
- One thing to note is that the **order_date** and **order_time** columns are set as **TEXT** datatypes.
- The Second thing to note is that **unit_price** and **total_price** columns are set as **DOUBLE** datatype.

#### 3.2 Data cleaning
As mentioned before the data is clean and well-maintained, there is not duplicates or empty values,
so we begin by standardizing the data:

Firstly we set the **order_date** and **order_time** columns into the standard format MYSQL uses,
and then we correct the datatype from **TEXT** to **DATE/TIME**.

Secondly, we set the **unit_price** and **total_price** columns as **DECIMAL** datatype.

```SQL
-- 2.STANDARDIZE DATA
UPDATE pizza_sales SET order_date = STR_TO_DATE(order_date, '%d/%m/%Y');
ALTER TABLE pizza_sales MODIFY order_date DATE;

UPDATE pizza_sales SET order_time = STR_TO_DATE(order_time, '%H:%i:%s');
ALTER TABLE pizza_sales MODIFY order_time TIME;

ALTER TABLE pizza_sales MODIFY unit_price DECIMAL(10, 2);
ALTER TABLE pizza_sales MODIFY total_price DECIMAL(10, 2);
```
#### 3.2 Data Normalization
WE organize the data into multiple related tables to reduce redundancy and improve data integrity.

1. Order Table: Tracks order information, like date and time.
2. Order Details Table: Links specific pizzas to an order, including quantity and prices.
3. Pizza Table: Stores pizza-specific details such as size, type, and ingredients.

```sql
CREATE TABLE Orders (
    order_id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
    order_date DATE NOT NULL,
    order_time TIME NOT NULL
);

CREATE TABLE OrderDetails (
    order_details_id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
    order_id INT NOT NULL,
    pizza_id INT NOT NULL,
    quantity INT NOT NULL,
    unit_price DECIMAL(10, 2) NOT NULL,
    total_price DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (order_id) REFERENCES Orders(order_id),
    FOREIGN KEY (pizza_id) REFERENCES Pizzas(pizza_id)
);

CREATE TABLE Pizzas (
    pizza_id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
    pizza_size VARCHAR(50) NOT NULL,
    pizza_category VARCHAR(100) NOT NULL,
    pizza_ingredients TEXT NOT NULL
);
```

<details>
<summary>And then insert the data into the corresponding tables:
</summary>

```sql
INSERT INTO Orders (order_date, order_time)
SELECT order_date, order_time
FROM pizza_sales;

INSERT INTO OrderDetails (order_id, pizza_id, quantity, unit_price, total_price)
SELECT 
    o.order_id, 
    p.pizza_id, 
    ps.quantity, 
    ps.unit_price,
    ps.total_price
FROM pizza_sales ps
JOIN Orders o ON ps.order_details_id = o.order_id
JOIN Pizzas p ON ps.order_details_id = p.pizza_id;

INSERT INTO Pizzas (pizza_size, pizza_category, pizza_ingredients)
SELECT pizza_size, pizza_category, pizza_ingredients
FROM pizza_sales;
```

</details>

#### 3.3 Data Transformation
<details>
<summary>In this section we fine-tune the data for visualization and analysis purposes.
</summary>

```sql
-- 4.TRANSFORM DATA
-- Rename values for visualization purposes
UPDATE Pizzas
SET pizza_size =
	CASE
		WHEN pizza_size = 'S' THEN 'Small'
        WHEN pizza_size = 'M' THEN 'Medium'
        WHEN pizza_size = 'L' THEN 'Large'
        WHEN pizza_size = 'XL' THEN 'XLarge'
        WHEN pizza_size = 'XXL' THEN 'XXLarge'
	END;

-- Add days_of_week column to Orders
ALTER TABLE Orders
ADD COLUMN days_of_week VARCHAR(20);

UPDATE Orders
SET days_of_week = DAYNAME(order_date)
WHERE order_id IS NOT NULL;

-- Add times_of_day column to Oorders
ALTER TABLE orders
ADD COLUMN times_of_day VARCHAR(20);

UPDATE Orders
SET times_of_day = (
	CASE 
		WHEN order_time BETWEEN '00:00:00' AND '11:59:59' THEN 'Morning' 
		WHEN order_time BETWEEN '12:00:00' AND '14:59:59' THEN 'Lunch'
		WHEN order_time BETWEEN '15:00:00' AND '17:59:59' THEN 'Afternoon' 
		WHEN order_time BETWEEN '18:00:00' AND '20:59:59' THEN 'Dinner'
		WHEN order_time BETWEEN '21:00:00' AND '23:59:59' THEN 'Late Evening' 
	END)
WHERE order_id IS NOT NULL;
```
</details>

