---
title: "Power BI: Measures"
datePublished: Thu Aug 29 2024 20:39:24 GMT+0000 (Coordinated Universal Time)
cuid: cm0fr092700040amhf1gaan1b
slug: power-bi-measures
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1724963818244/8b21dc21-19ff-4367-9693-17f0416321bf.jpeg
tags: data-science, data-analysis, business-intelligence, powerbi

---

In this blog post, we’ll dive into what measures are, how to create them, and demonstrate their use with examples based on a sales dataset.

## What are Measures in Power BI?

Measures are dynamic calculations that adjust based on the context of your data. Unlike calculated columns, which perform row-by-row calculations and store the result in the data model, measures calculate results on the fly as you interact with your reports. This makes them incredibly flexible and powerful for data analysis.

## How to Create a Measure in Power BI

Creating a measure in Power BI is straightforward. Follow these steps:

1. **Open Power BI Desktop**: Start by launching Power BI Desktop and loading your dataset.
    
2. **Select a Table**: Navigate to the “Data” view and select the table where you want to create the measure. Typically, you would choose a table that relates closely to the data your measure will calculate.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1724962069367/26e2ccf1-2633-4c2f-9bc1-986bbcc6e8d5.png align="center")
    
3. **Create the Measure**:
    
    * Right-click on the table name in the Fields pane.
        
    * Select **New Measure** from the context menu.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1724962178413/b8955cb3-23a3-4111-937e-75453aab10ae.png align="center")
        
    * In the formula bar, start typing your DAX expression. For example, to create a measure for Total Sales, you might write:
        
    
    ```excel
    Total Sales = SUM(Orders[TotalAmount])
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1724964225161/d6d07b7f-6b2a-4b13-a95e-de116aadde71.png align="center")
    
4. **Name Your Measure**: The first part of the DAX expression before the equals sign (`=`) is the name of your measure. Make sure to choose a name that clearly describes what the measure calculates.
    
5. **Use the Measure in a Visual**: Once the measure is created, you can drag it into a report visual, such as a table, chart, or matrix, and it will calculate based on the data context of that visual.
    

## Dataset Overview

Let’s briefly review the dataset ([Download it from here](https://y15w7-my.sharepoint.com/:x:/g/personal/me_mbvrk_onmicrosoft_com/EQ3lhi9e9LRKmRs4kwMFT8cBT4OYU79rqIT5rxFcBk4rrA?e=ncueOT)) we’ll use for our examples. The dataset contains the following tables:

* **Customers**: Contains customer information such as ID, name, gender, country, city, age, and marital status.
    
* **Products**: Holds product details including title, category, unit price, weight, and manufacturer.
    
* **Stocks**: Displays current stock levels for each product.
    
* **Orders**: Captures order transactions including customer ID, product ID, quantity, unit price, and total amount.
    

## Examples of Measures in Power BI

### Example 1: Total Sales

To calculate the total sales, we’ll create a measure that sums the `TotalAmount` column in the `Orders` table.

```excel
Total Sales = SUM(Orders[TotalAmount])
```

This measure will give you the total sales amount for all orders.

### Example 2: Average Order Value

To understand the average value of an order, we can create the following measure:

```excel
Average Order Value = AVERAGE(Orders[TotalAmount])
```

This measure calculates the average amount per order by using the `AVERAGE` function on the `TotalAmount` column.

### Example 3: Total Quantity Sold

To determine how many products have been sold, you can create a measure that sums up the quantity:

```excel
Total Quantity Sold = SUM(Orders[Quantity])
```

This measure sums the `Quantity` column to provide the total number of items sold.

### Example 4: Total Sales by Category

To analyze total sales by product category, create a measure that multiplies the quantity sold by the unit price for each product:

```excel
Total Sales by Category = SUMX(RELATEDTABLE(Products), Orders[Quantity] * Products[UnitPrice])
```

**OR**

```excel
Total Sales by Category = SUMX(Products, Orders[Quantity] * Products[UnitPrice])
```

`SUMX` iterates over each product related to the orders and calculates the total sales by category.

### Example 5: Number of Orders

Finally, to count the number of orders, use the following measure:

```excel
Number of Orders = COUNTROWS(Orders)
```

This measure counts the number of rows in the `Orders` table, representing the total number of orders made.

Now, once the measure is created, you can drag it into a report visual.

## Conclusion

Creating and using measures in Power BI is essential for any data analyst or business intelligence professional. Measures allow for dynamic, context-sensitive calculations that bring your data to life.