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
  - [3. Prepare Phase](#3-prepare-phase)
  - [4. Process and Analyse Phase](#4-process-and-analyse-phase)

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
This Pizza Sales dataset contains 12 columns:
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
SQL which be used to create a database Schema.

Data analysis was conducted using Python for statistical computations and Tableau for visualizations. The methodologies included correlation analysis, linear regression, and cluster analysis to identify patterns and draw insights.

## 3. Prepare Phase

## 4. Process and Analyse Phase