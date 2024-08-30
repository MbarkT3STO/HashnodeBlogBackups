---
title: "Power BI DAX: LOOKUPVALUE Function"
datePublished: Fri Aug 30 2024 11:59:32 GMT+0000 (Coordinated Universal Time)
cuid: cm0gnvjyl00000al52kiv7s10
slug: power-bi-dax-lookupvalue-function
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1725017203767/5bab4996-80c2-40f1-bf89-788bc99970ce.jpeg
tags: data-science, data-analysis, business-intelligence, powerbi

---

In this blog post, we’ll explore how to use the `LOOKUPVALUE` function in Power BI DAX, with practical examples based on a sales dataset.

## What is the `LOOKUPVALUE` Function in DAX?

The `LOOKUPVALUE` function in DAX allows you to search for a specific value in a table based on one or more search conditions. It’s similar to the `VLOOKUP` function in Excel but much more powerful and flexible, particularly in the context of relational data models in Power BI.

The basic syntax for `LOOKUPVALUE` is:

```excel
LOOKUPVALUE(<result_columnName>, <search_columnName>, <search_value>, [<search_columnName>, <search_value>]…)
```

* `<result_columnName>`: The column from which you want to return a value.
    
* `<search_columnName>`: The column you want to search.
    
* `<search_value>`: The value you want to find in the search column.
    
* `[<search_columnName>, <search_value>]`: Optional additional search conditions.
    

## **Dataset Overview**

Let’s briefly review the dataset ([**Download it from here)**](https://y15w7-my.sharepoint.com/:x:/g/personal/me_mbvrk_onmicrosoft_com/EQ3lhi9e9LRKmRs4kwMFT8cBT4OYU79rqIT5rxFcBk4rrA?e=ncueOT) we’ll use for our examples. The dataset contains the following tables:

* **Customers**: Contains customer information such as ID, name, gender, country, city, age, and marital status.
    
* **Products**: Holds product details including title, category, unit price, weight, and manufacturer.
    
* **Stocks**: Displays current stock levels for each product.
    
* **Orders**: Captures order transactions including customer ID, product ID, quantity, unit price, and total amount.
    

## Examples

### Example 1: Retrieve Customer Name Based on Customer ID

Suppose you want to add a column to the `Orders` table that displays the First Name of the customer who placed each order. You can use the `LOOKUPVALUE` function to achieve this by looking up the customer’s first name from the `Customers` table.

```excel
Customer First Name = 
LOOKUPVALUE(
    Customers[FirstName], 
    Customers[CustomerID], Orders[CustomerID]
)
```

In this example, `LOOKUPVALUE` retrieves the first name from the `Customers` table where the `CustomerID` matches the `CustomerID` in the `Orders` table.

### Example 2: Fetch Product Category Based on Product ID

If you need to display the product category in the `Orders` table based on the `ProductID`, you can use the `LOOKUPVALUE` function as follows:

```excel
Product Category = 
LOOKUPVALUE(
    Products[Category], 
    Products[ProductID], Orders[ProductID]
)
```

This formula retrieves the `Category` from the `Products` table where the `ProductID` matches the `ProductID` in the `Orders` table.

### Example 3: Calculate Discounted Price Based on Product and Customer Conditions

You might want to apply a special discount for customers from a specific country when they purchase products from a specific category. For instance, suppose customers from Canada get a 10% discount on "Electronics" products. You can achieve this with the following DAX expression:

```excel
Discounted Price = 
IF(
    LOOKUPVALUE(Customers[Country], Customers[CustomerID], Orders[CustomerID]) = "Canada" && 
    LOOKUPVALUE(Products[Category], Products[ProductID], Orders[ProductID]) = "Electronics",
    Orders[UnitPrice] * 0.9,
    Orders[UnitPrice]
)
```

This formula uses `LOOKUPVALUE` to check the customer’s country and product category. If the customer is from Canada and the product category is "Electronics", the formula applies a 10% discount; otherwise, it returns the original unit price.

### Example 4: Combine Multiple Conditions

If you need to look up a value based on multiple conditions, `LOOKUPVALUE` allows you to do so. For instance, retrieving a customer’s family name based on both `CustomerID` and `Country`:

```excel
Customer Family Name with Country = 
LOOKUPVALUE(
    Customers[FamilyName], 
    Customers[CustomerID], Orders[CustomerID],
    Customers[Country], "United States"
)
```

This formula looks up the family name of a customer only if the `CustomerID` matches and the `Country` is "United States".

## Conclusion

The `LOOKUPVALUE` function in DAX is an incredibly powerful tool for retrieving data across related tables in Power BI. Whether you're combining customer details with order data, fetching product information, or applying complex conditions, `LOOKUPVALUE` provides the flexibility needed for dynamic and context-aware data models.