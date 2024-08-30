---
title: "Power BI DAX: SUMX, AVERAGEX, and COUNTX Functions"
seoDescription: "Learn how to use Power BI DAX functions: SUMX, AVERAGEX, and COUNTX with practical examples"
datePublished: Fri Aug 30 2024 17:11:38 GMT+0000 (Coordinated Universal Time)
cuid: cm0gz0x7f005j09kx8ft37yh0
slug: power-bi-dax-sumx-averagex-and-countx-functions
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1725037294164/6e23a1bd-7b16-4616-904f-6ec41aebee6b.jpeg
tags: data-science, data-analysis, business-intelligence, powerbi

---

In this blog post, we’ll dive into the `SUMX`, `AVERAGEX`, and `COUNTX` functions, explaining how they work and providing practical examples based on a sales dataset.

## What Are the `SUMX`, `AVERAGEX`, and `COUNTX` Functions in DAX?

The `X` functions in DAX are iterator functions that operate on a row-by-row basis over a table. Here’s a brief overview:

* `SUMX`: Calculates the sum of an expression evaluated for each row in a table.
    
* `AVERAGEX`: Calculates the average of an expression evaluated for each row in a table.
    
* `COUNTX`: Counts the number of non-blank results for an expression evaluated for each row in a table.
    

### Basic Syntax

* `SUMX`:
    
    ```excel
    SUMX(<table>, <expression>)
    ```
    
* `AVERAGEX`:
    
    ```excel
    AVERAGEX(<table>, <expression>)
    ```
    
* `COUNTX`:
    
    ```excel
    COUNTX(<table>, <expression>)
    ```
    
* `<table>`: The table in which the calculation is performed.
    
* `<expression>`: The expression that is evaluated for each row in the table.
    

## **Dataset Overview**

Let’s briefly review the dataset ([**Download it from here)**](https://y15w7-my.sharepoint.com/:x:/g/personal/me_mbvrk_onmicrosoft_com/EQ3lhi9e9LRKmRs4kwMFT8cBT4OYU79rqIT5rxFcBk4rrA?e=ncueOT) we’ll use for our examples. The dataset contains the following tables:

* **Customers**: Contains customer information such as ID, name, gender, country, city, age, and marital status.
    
* **Products**: Holds product details including title, category, unit price, weight, and manufacturer.
    
* **Stocks**: Displays current stock levels for each product.
    
* **Orders**: Captures order transactions including customer ID, product ID, quantity, unit price, and total amount.
    

## Examples

### Example 1: Calculate Total Revenue per Order Using `SUMX`

Suppose you want to calculate the total revenue for each order by multiplying the quantity of each product by its unit price and then summing up these values across all orders. You can use the `SUMX` function for this:

```excel
Total Revenue = 
SUMX(
    Orders, 
    Orders[Quantity] * Orders[UnitPrice]
)
```

In this example, `SUMX` iterates over each row in the `Orders` table, multiplies the `Quantity` by the `UnitPrice`, and then sums the results to give the total revenue.

### Example 2: Calculate the Average Order Value Using `AVERAGEX`

To calculate the average value of an order, you can use the `AVERAGEX` function, which will average the total revenue per order:

```excel
Average Order Value = 
AVERAGEX(
    Orders, 
    Orders[Quantity] * Orders[UnitPrice]
)
```

Here, `AVERAGEX` first calculates the revenue for each order using the same expression as in the `SUMX` example, then calculates the average of these revenues.

### Example 3: Count the Number of Orders Above a Certain Value Using `COUNTX`

Suppose you want to count the number of orders where the total order value exceeds $500. You can achieve this using the `COUNTX` function:

```excel
Count of High-Value Orders = 
COUNTX(
    Orders, 
    IF(Orders[Quantity] * Orders[UnitPrice] > 500, 1, BLANK())
)
```

In this example, `COUNTX` evaluates each row in the `Orders` table. If the total order value (calculated as `Quantity` \* `UnitPrice`) exceeds $500, it returns 1; otherwise, it returns `BLANK`. `COUNTX` then counts the non-blank results.

### Example 4: Calculate Stock Value Using `SUMX`

To calculate the total stock value of all products (i.e., the sum of each product's stock quantity multiplied by its unit price), you can use:

```excel
Total Stock Value = 
SUMX(
    Stocks, 
    Stocks[Quantity] * Products[UnitPrice]
)
```

This formula multiplies each product’s stock level / `Quantity` by its `UnitPrice` and sums the results to provide the total stock value.

## Conclusion

The `SUMX`, `AVERAGEX`, and `COUNTX` functions in DAX are incredibly powerful tools for performing row-by-row calculations in Power BI. These functions allow you to aggregate data in more complex ways than standard aggregation functions, enabling more nuanced analysis and insights.