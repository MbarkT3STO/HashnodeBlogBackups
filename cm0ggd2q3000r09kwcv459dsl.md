---
title: "Power BI: Calculated Columns"
datePublished: Fri Aug 30 2024 08:29:12 GMT+0000 (Coordinated Universal Time)
cuid: cm0ggd2q3000r09kwcv459dsl
slug: power-bi-calculated-columns
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1725005219588/4094b7c7-6bd0-4999-9da8-c0fa308379e6.jpeg
tags: data-science, data-analysis, business-intelligence, powerbi

---

In this blog post, we’ll dive into what calculated columns are, how to create them, and provide practical examples using a sales dataset.

## What are Calculated Columns in Power BI?

Calculated columns are new columns you can create in your data model using Data Analysis Expressions (DAX). These columns are computed for each row in a table and stored as part of the dataset. Calculated columns are particularly useful for adding new data fields that don't exist in your source data, such as creating a full name by combining first and last names or calculating a profit margin based on sales and cost data.

## How to Create a Calculated Column in Power BI

Creating a calculated column in Power BI is straightforward. Follow these steps:

1. **Open Power BI Desktop**: Start by launching Power BI Desktop and loading your dataset.
    
2. **Select a Table**: Navigate to the “Data” view and select the table where you want to add the calculated column.
    
3. **Create the Calculated Column**:
    
    * In the Fields pane, right-click on the table name and choose **New Column**.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1725005532645/99c4b98c-b47b-4017-bdbf-8b72ff33ea0a.png align="center")
        
    * A formula bar will appear where you can enter your DAX expression. For instance, to create a calculated column for the full name of customers, you might write:
        
    
    ```excel
    Full Name = Customers[FirstName] & " " & Customers[FamilyName]
    ```
    
4. **Name Your Column**: The part of the DAX expression before the equals sign (`=`) is the name of your calculated column. Choose a name that clearly describes the content of the column.
    
5. **Use the Column**: Once created, the calculated column will appear in the table and can be used in your reports just like any other column.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1725005742382/ab6ad152-f2b3-45cf-b583-34cd0af1c59d.png align="center")
    

## Dataset Overview

Let’s briefly review the dataset ([**Download it from here**](https://y15w7-my.sharepoint.com/:x:/g/personal/me_mbvrk_onmicrosoft_com/EQ3lhi9e9LRKmRs4kwMFT8cBT4OYU79rqIT5rxFcBk4rrA?e=ncueOT)) we’ll use for our examples. The dataset contains the following tables:

* **Customers**: Contains customer information such as ID, name, gender, country, city, age, and marital status.
    
* **Products**: Holds product details including title, category, unit price, weight, and manufacturer.
    
* **Stocks**: Displays current stock levels for each product.
    
* **Orders**: Captures order transactions including customer ID, product ID, quantity, unit price, and total amount.
    

## Examples of Calculated Columns in Power BI

### Example 1: Full Name

Let’s start by creating a calculated column that combines the first name and family name of each customer into a full name.

```excel
Full Name = Customers[FirstName] & " " & Customers[FamilyName]
```

This calculated column concatenates the `FirstName` and `FamilyName` columns with a space in between, providing a complete name for each customer.

### Example 2: Profit Margin

Suppose we want to calculate the profit margin for each product in the `Orders` table. Assuming the cost is 70% of the unit price, the formula might look like this:

```excel
Profit Margin = Orders[UnitePrice] * 0.3
```

This formula creates a new column that represents the profit margin for each order by multiplying the unit price by 30%.

### Example 3: Year of Order

If you need to extract the year from the order date, you can create a calculated column that does this:

```excel
Order Year = YEAR(Orders[DateAndTime])
```

This column will extract the year from the `DateAndTime` field, providing a clearer understanding of when orders were placed.

### Example 4: Product Category & Title

To simplify reporting, you may want to create a single column that combines the product category and title:

```excel
Product Info = Products[Category] & " - " & Products[Title]
```

This calculated column merges the `Category` and `Title` fields from the `Products` table, making it easier to analyze data by category and product name together.

### Example 5: Order Quantity Classification

You might want to classify orders based on the quantity ordered, creating categories like "Small", "Medium", and "Large":

```excel
Order Size = 
IF(Orders[Quantity] <= 10, "Small", 
    IF(Orders[Quantity] <= 50, "Medium", 
    "Large"))
```

This DAX formula categorizes each order based on the `Quantity` column, creating a new field that labels each order as "Small," "Medium," or "Large".

## Conclusion

Calculated columns are a powerful feature in Power BI that enable you to create new data fields tailored to your analysis needs.