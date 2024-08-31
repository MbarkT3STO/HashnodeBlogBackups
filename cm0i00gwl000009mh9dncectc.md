---
title: "Power BI DAX: SUMMARIZE Function"
datePublished: Sat Aug 31 2024 10:27:03 GMT+0000 (Coordinated Universal Time)
cuid: cm0i00gwl000009mh9dncectc
slug: power-bi-dax-summarize-function
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1725097584895/13c9401b-e093-4025-85bf-a3009b299804.jpeg
tags: data-science, data-analysis, business-intelligence, powerbi

---

In this blog post, we’ll explore the `SUMMARIZE` function in DAX, explain how it works, and provide practical examples using a sales dataset. Each example will also show the resulting tables to give you a clearer understanding of how `SUMMARIZE` can be applied in real-world scenarios.

## What is the `SUMMARIZE` Function in DAX?

The `SUMMARIZE` function in DAX allows you to create a new table by grouping columns from an existing table and calculating aggregates such as sums, averages, counts, and more. It’s similar to SQL’s `GROUP BY` statement and is often used to create intermediate tables for further analysis.

### Basic Syntax

The basic syntax for the `SUMMARIZE` function is:

```excel
SUMMARIZE(
    <table>,
    <groupBy_columnName1>, <groupBy_columnName2>, ...,
    [<name1>, <expression1>], [<name2>, <expression2>], ...
)
```

* `<table>`: The table to group and summarize.
    
* `<groupBy_columnName1>, <groupBy_columnName2>, ...`: The columns by which to group the data.
    
* `[<name1>, <expression1>]`: Optional name-value pairs for the columns and expressions to calculate.
    

## **Dataset Overview**

Let’s briefly review the dataset ([**Download it from here)**](https://y15w7-my.sharepoint.com/:x:/g/personal/me_mbvrk_onmicrosoft_com/EQ3lhi9e9LRKmRs4kwMFT8cBT4OYU79rqIT5rxFcBk4rrA?e=ncueOT) we’ll use for our examples. The dataset contains the following tables:

* **Customers**: Contains customer information such as ID, name, gender, country, city, age, and marital status.
    
* **Products**: Holds product details including title, category, unit price, weight, and manufacturer.
    
* **Stocks**: Displays current stock levels for each product.
    
* **Orders**: Captures order transactions including customer ID, product ID, quantity, unit price, and total amount.
    

## Where to Write the DAX Code and View the Results of the `SUMMARIZE` Function

To use the `SUMMARIZE` function in Power BI, you'll typically write your DAX code in one of the following locations:

#### 1\. **New Table**

If you want to create a new table that displays the results of your `SUMMARIZE` function, follow these steps:

1. **Go to the "Modeling" tab** in Power BI Desktop.
    
2. **Click on "New Table."**
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1725099035042/f82049fa-fe2d-4d4d-ac31-159070b7f3b5.png align="center")
    
3. In the formula bar that appears, write your `SUMMARIZE` function.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1725099169887/0ebfeeec-11f2-4571-9653-53af25b598af.png align="center")
    
4. **Press Enter** to create the table. The new table will appear in the "Fields" pane, and you can use it in your reports like any other table.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1725099277896/979ed6cf-d5c4-4800-b73e-c30b5be5397d.png align="center")
    

#### 2\. **New Measure**

If you want to use the `SUMMARIZE` function as part of a larger calculation or measure:

1. **Go to the "Modeling" tab** in Power BI Desktop.
    
2. **Click on "New Measure."**
    
3. In the formula bar, write your DAX expression that includes the `SUMMARIZE` function.
    
    ```excel
    Total Revenue by Category = 
    SUMX(
        SUMMARIZE(
            Orders,
            Products[Category],
            "Category Revenue", SUM(Orders[TotalAmount])
        ),
        [Category Revenue]
    )
    ```
    
4. **Press Enter** to create the measure. This measure will be available in the "Fields" pane for use in visualizations.
    

#### 3\. **DAX Queries in Power BI**

For advanced users who want to test their DAX queries directly:

1. **Open the "DAX Studio" tool** if installed (an external tool that connects to your Power BI model, you can [download it from here](https://daxstudio.org/)).
    
2. Write your `SUMMARIZE` query in DAX Studio and execute it to view the results directly in the tool.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1725099943162/fa017e80-f9ff-4e3c-b17d-c652d1c57b63.png align="center")
    

This method is particularly useful for testing and debugging complex DAX queries outside of the Power BI interface.

## Examples

### Example 1: Summarizing Total Revenue by Product Category

Let’s start with a basic example. Suppose you want to calculate the total revenue for each product category. You can use the `SUMMARIZE` function to group the `Orders` table by `Category` from the `Products` table and calculate the sum of `TotalAmount`.

```excel
Category Revenue Summary = 
SUMMARIZE(
    Orders,
    Products[Category],
    "Total Revenue", SUM(Orders[TotalAmount])
)
```

#### Resulting Table:

| Category | Total Revenue |
| --- | --- |
| Appliances | 45888993.57 |
| Electronics | 105443116.75 |
| Gadgets | 86784971.00 |

In this example, the `SUMMARIZE` function groups the `Orders` table by `Category` and calculates the total revenue for each category.

### Example 2: Counting Orders by Customer Country

Suppose you want to know how many orders were placed by customers from each country. You can summarize the `Orders` table by `Country` from the `Customers` table and count the number of orders.

```excel
Orders by Country = 
SUMMARIZE(
    Orders,
    Customers[Country],
    "Order Count", COUNT(Orders[ID])
)
```

#### Resulting Table:

| Country | Order Count |
| --- | --- |
| Moldova | 100 |
| Iraq | 100 |
| Italy | 200 |
| Zambia | 100 |
| Israel | 100 |
| Slovakia | 100 |
| Malawi | 200 |
| New Zealand | 100 |
| Guyana | 200 |
| Albania | 100 |
| Ghana | 100 |
| Bangladesh | 200 |
| Canada | 100 |
| Malaysia | 100 |
| Swaziland | 200 |
| Sri Lanka | 200 |
| France | 100 |
| Panama | 100 |
| Cuba | 100 |
| Ukraine | 100 |
| Fiji | 100 |
| Sierra Leone | 100 |
| Guatemala | 100 |
| Denmark | 200 |
| Suriname | 100 |
| Finland | 100 |
| Luxembourg | 100 |
| Argentina | 100 |
| Bahrain | 100 |
| Afghanistan | 100 |
| Nicaragua | 100 |
| Saudi Arabia | 100 |
| Nigeria | 100 |
| Australia | 100 |
| Austria | 100 |
| Netherlands | 100 |
| Botswana | 100 |
| Mongolia | 100 |
| Uruguay | 200 |
| Tunisia | 100 |
| Brazil | 100 |
| Croatia | 100 |

This example shows how to group orders by customer country and count the number of orders placed by customers from each country.

### Example 3: Calculating Average Unit Price by Product Category

If you’re interested in finding out the average unit price of products in each category, you can use the `SUMMARIZE` function to group by `Category` and calculate the average of `UnitPrice`.

```excel
Average Price by Category = 
SUMMARIZE(
    Products,
    Products[Category],
    "Average Unit Price", AVERAGE(Products[UnitPrice])
)
```

#### Resulting Table:

| Category | Average Unit Price |
| --- | --- |
| Appliances | 715.86 |
| Electronics | 1123.33 |
| Gadgets | 859.36 |

This example groups products by category and calculates the average unit price for each category.

### Example 4: Summarizing Stock Levels by Product Manufacturer

Finally, let’s calculate the total stock level for each product manufacturer. We’ll group the `Stocks` table by `Manufacturer` from the `Products` table and sum the `StockLevel`.

```excel
Stock by Manufacturer = 
SUMMARIZE(
    Stocks,
    Products[Manufacturer],
    "Total Stock", SUM(Stocks[Quantity])
)
```

#### Resulting Table:

| Manufacturer | Total Stock |
| --- | --- |
| CoolTech | 83 |
| SoundMasters | 51 |
| TechVision | 77 |
| GadgetCo | 63 |
| TechLand | 33 |
| VisionTech | 71 |
| GamerTech | 54 |
| ProMedia | 96 |
| TechCo | 184 |
| BrewMasters | 38 |
| PrintTech | 78 |
| TechPro | 17 |
| GameZone | 29 |
| AudioTech | 23 |
| CleanTech | 111 |
| FitTech | 61 |
| MediaTech | 16 |
| ColdMasters | 30 |
| PhoneCo | 68 |
| ScienceTech | 16 |
| NavTech | 44 |
| TechStand | 79 |

This example shows how to group stock levels by manufacturer and calculate the total stock for each manufacturer.

## Conclusion

The `SUMMARIZE` function in Power BI DAX is an essential tool for creating summary tables and performing aggregate calculations. Whether you’re analyzing sales revenue, counting orders, calculating averages, or summarizing stock levels, `SUMMARIZE` allows you to group data by one or more columns and calculate meaningful aggregates.