---
title: "Power BI DAX: SWITCH Function"
datePublished: Fri Aug 30 2024 17:51:52 GMT+0000 (Coordinated Universal Time)
cuid: cm0h0gnpq001t09mc5odu7fct
slug: power-bi-dax-switch-function
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1725038408697/ece24ca0-0c00-46ca-8045-0d3748da51c2.jpeg
tags: data-science, data-analysis, business-intelligence, powerbi

---

In this blog post, we’ll explore the `SWITCH` function in detail, explaining how it works and providing practical examples based on a sales dataset.

## What is the `SWITCH` Function in DAX?

The `SWITCH` function in DAX evaluates an expression against a list of values and returns a result that corresponds to the first matching value. It’s a more efficient alternative to nested `IF` statements, particularly when dealing with multiple conditions.

### Basic Syntax

The basic syntax of the `SWITCH` function is:

```excel
SWITCH(<expression>, <value1>, <result1>, <value2>, <result2>, ..., [<else>])
```

* `<expression>`: The value or expression to test.
    
* `<value1>, <value2>, ...`: The values to match against the expression.
    
* `<result1>, <result2>, ...`: The result to return when a match is found.
    
* `[<else>]`: The result to return if no match is found (optional).
    

## **Dataset Overview**

Let’s briefly review the dataset ([**Download it from here)**](https://y15w7-my.sharepoint.com/:x:/g/personal/me_mbvrk_onmicrosoft_com/EQ3lhi9e9LRKmRs4kwMFT8cBT4OYU79rqIT5rxFcBk4rrA?e=ncueOT) we’ll use for our examples. The dataset contains the following tables:

* **Customers**: Contains customer information such as ID, name, gender, country, city, age, and marital status.
    
* **Products**: Holds product details including title, category, unit price, weight, and manufacturer.
    
* **Stocks**: Displays current stock levels for each product.
    
* **Orders**: Captures order transactions including customer ID, product ID, quantity, unit price, and total amount.
    

## Examples

### Example 1: Categorizing Customers Based on Age Group

Suppose you want to categorize customers into different age groups based on their age. You can use the `SWITCH` function to create a calculated column that assigns each customer to an age group:

```excel
Age Group = 
SWITCH(
    TRUE(), 
    Customers[Age] < 18, "Under 18",
    Customers[Age] <= 35, "18-35",
    Customers[Age] <= 50, "36-50",
    Customers[Age] > 50, "51+",
    "Unknown"
)
```

In this example, the `SWITCH` function evaluates the condition `TRUE()`, which always returns true, allowing the function to check each condition in sequence. The customer is categorized into the appropriate age group based on their age.

### Example 2: Assigning Product Categories Based on Unit Price

Imagine you want to classify products into different categories based on their unit price. For example, products under $20 could be classified as "Budget", between $20 and $100 as "Standard", and above $100 as "Premium":

```excel
Price Category = 
SWITCH(
    TRUE(),
    Products[UnitPrice] < 20, "Budget",
    Products[UnitPrice] <= 100, "Standard",
    Products[UnitPrice] > 100, "Premium",
    "Undefined"
)
```

This DAX expression categorizes each product based on its `UnitPrice`, making it easier to analyze products in different pricing segments.

### Example 3: Labeling Orders Based on Quantity

To classify orders based on the quantity ordered, you can use the `SWITCH` function to create labels such as "Small Order", "Medium Order", and "Large Order":

```excel
Order Size = 
SWITCH(
    TRUE(),
    Orders[Quantity] <= 10, "Small Order",
    Orders[Quantity] <= 50, "Medium Order",
    Orders[Quantity] > 50, "Large Order",
    "Unknown"
)
```

Here, `SWITCH` evaluates the order quantity and assigns the appropriate label to each order based on the conditions provided.

### Example 4: Applying Conditional Discounts Based on Product Category and Customer Country

You might want to apply different discount rates based on the product category and customer country. For example, customers from the USA might receive a 5% discount on "Electronics" and a 10% discount on "Clothing":

```excel
Discount Rate = 
SWITCH(
    TRUE(),
    RELATED(Products[Category]) = "Electronics" && RELATED(Customers[Country]) = "USA", 0.05,
    RELATED(Products[Category]) = "Clothing" && RELATED(Customers[Country]) = "USA", 0.10,
    0
)
```

This expression applies the specified discount rate based on the product category and customer country, returning 0 if no conditions are met.

### Example 5: Creating a Sales Performance Indicator

To create a performance indicator based on sales revenue, you can use the `SWITCH` function to categorize performance into "Low", "Medium", and "High" tiers:

```excel
Sales Performance = 
SWITCH(
    TRUE(),
    Orders[TotalAmount] < 500, "Low",
    Orders[TotalAmount] <= 2000, "Medium",
    Orders[TotalAmount] > 2000, "High",
    "Unknown"
)
```

This DAX formula categorizes the sales performance of each order based on the total revenue, providing a quick overview of order values.

## Conclusion

The `SWITCH` function in DAX is an essential tool for implementing complex conditional logic in Power BI, offering a cleaner and more readable alternative to nested `IF` statements. Whether you’re categorizing customers by age, classifying products by price, or applying conditional discounts, the `SWITCH` function can simplify your DAX expressions and make your data models more efficient.