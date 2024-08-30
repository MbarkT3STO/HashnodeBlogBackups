---
title: "Power BI DAX: IF Function"
datePublished: Fri Aug 30 2024 10:39:25 GMT+0000 (Coordinated Universal Time)
cuid: cm0gl0iv2000w09mf6k6caw3p
slug: power-bi-dax-if-function
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1725013722700/2c04acb5-c0c0-43cd-aa4f-1d4f84c32a19.jpeg
tags: data-science, data-analysis, business-intelligence, powerbi

---

In this blog post, we’ll explore the `IF` function in detail, explain how to use it, and provide practical examples based on a sales dataset.

## What is the `IF` Function in DAX?

The `IF` function in DAX is used to test a condition and return one value if the condition is true and another value if the condition is false. It’s similar to the `IF` function in Excel but tailored for use in Power BI’s data modeling environment.

The basic syntax for `IF` is:

```excel
IF(<logical_test>, <value_if_true>, [<value_if_false>])
```

* `<logical_test>`: The condition you want to evaluate.
    
* `<value_if_true>`: The value to return if the condition is true.
    
* `[<value_if_false>]`: The value to return if the condition is false (optional).
    

## **Dataset Overview**

Let’s briefly review the dataset ([**Download it from here**)](https://y15w7-my.sharepoint.com/:x:/g/personal/me_mbvrk_onmicrosoft_com/EQ3lhi9e9LRKmRs4kwMFT8cBT4OYU79rqIT5rxFcBk4rrA?e=ncueOT) we’ll use for our examples. The dataset contains the following tables:

* **Customers**: Contains customer information such as ID, name, gender, country, city, age, and marital status.
    
* **Products**: Holds product details including title, category, unit price, weight, and manufacturer.
    
* **Stocks**: Displays current stock levels for each product.
    
* **Orders**: Captures order transactions including customer ID, product ID, quantity, unit price, and total amount.
    

## Examples

### Example 1: Categorizing Order Sizes

Suppose you want to categorize orders as "Small", "Medium", or "Large" based on the quantity ordered. You can use the `IF` function to create a calculated column that classifies each order:

```excel
Order Size = 
IF(Orders[Quantity] <= 10, "Small", 
    IF(Orders[Quantity] <= 50, "Medium", 
    "Large"))
```

This formula uses nested `IF` functions to classify each order into one of three categories based on the quantity ordered.

### Example 2: Flagging High-Value Orders

You might want to identify high-value orders, where the total amount exceeds $1,000. This can be achieved using the `IF` function:

```excel
High-Value Order = 
IF(Orders[TotalAmount] > 1000, "Yes", "No")
```

In this example, the `IF` function checks if the `TotalAmount` for an order is greater than 1,000. If true, it returns "Yes"; otherwise, it returns "No".

### Example 3: Handling Null or Missing Data

Data often contains null or missing values that need to be handled in your analysis. You can use the `IF` function to replace null values with a default value. For instance, if a customer’s city is missing, you might want to fill it with "Unknown":

```excel
City = 
IF(ISBLANK(Customers[City]), "Unknown", Customers[City])
```

Here, the `ISBLANK` function checks if the `City` field is null. If it is, the `IF` function returns "Unknown"; otherwise, it returns the actual city name.

### Example 4: Applying Discounts Based on Quantity

If your business offers discounts based on the quantity ordered, you can use the `IF` function to calculate the discounted price. For example:

```excel
Discounted Price = 
IF(Orders[Quantity] > 50, Orders[UnitPrice] * 0.9, Orders[UnitPrice])
```

This formula checks if the quantity ordered is greater than 50. If it is, a 10% discount is applied to the unit price; otherwise, the original unit price is used.

### Example 5: Calculating Age Categories

To categorize customers into age groups, you can use the `IF` function in combination with other logical operators:

```excel
Age Group = 
IF(Customers[Age] < 18, "Under 18", 
    IF(Customers[Age] <= 35, "18-35", 
    IF(Customers[Age] <= 50, "36-50", "51+")))
```

This formula categorizes customers into four age groups: "Under 18", "18-35", "36-50", and "51+" based on their age.

## Conclusion

The `IF` function in DAX is a fundamental tool for implementing conditional logic in your Power BI reports. Whether you're categorizing data, flagging important records, handling missing values, or applying custom calculations, `IF` enables you to tailor your analysis to meet specific needs.