---
title: "Power BI DAX: Variables"
datePublished: Sat Aug 31 2024 08:43:39 GMT+0000 (Coordinated Universal Time)
cuid: cm0hwbi1l000308ld2nso3vg3
slug: power-bi-dax-variables
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1725093224333/c55e84d9-bb4a-4487-8219-65ab60cd6533.jpeg
tags: data-science, data-analysis, business-intelligence, powerbi

---

In this blog post, we’ll explore the concept of variables in DAX, explaining how to define and use them with practical examples based on a sales dataset.

## What Are Variables in DAX?

Variables in DAX are used to store the result of an expression so that you can reuse it multiple times within the same measure, calculated column, or table. This can help to simplify complex calculations and improve the performance of your DAX queries.

### Basic Syntax

The basic syntax for defining and using variables in DAX is:

```excel
VAR <variable_name> = <expression>
RETURN <result>
```

* `VAR <variable_name>`: Defines a variable with a name and assigns it an expression.
    
* `RETURN <result>`: Specifies the expression or calculation that will use the variables defined.
    

## **Dataset Overview**

Let’s briefly review the dataset ([**Download it from here)**](https://y15w7-my.sharepoint.com/:x:/g/personal/me_mbvrk_onmicrosoft_com/EQ3lhi9e9LRKmRs4kwMFT8cBT4OYU79rqIT5rxFcBk4rrA?e=ncueOT) we’ll use for our examples. The dataset contains the following tables:

* **Customers**: Contains customer information such as ID, name, gender, country, city, age, and marital status.
    
* **Products**: Holds product details including title, category, unit price, weight, and manufacturer.
    
* **Stocks**: Displays current stock levels for each product.
    
* **Orders**: Captures order transactions including customer ID, product ID, quantity, unit price, and total amount.
    

## Examples

### Example 1: Simplifying Revenue Calculations

Suppose you need to calculate the total revenue for each order by multiplying the quantity by the unit price. Without variables, you might write a DAX expression like this:

```excel
Total Revenue = 
SUMX(Orders, Orders[Quantity] * Orders[UnitPrice])
```

While this expression is straightforward, imagine if you had to use this calculation multiple times in a more complex measure. Using variables, you can store this calculation in a variable and reuse it:

```excel
Total Revenue = 
VAR Revenue = Orders[Quantity] * Orders[UnitPrice]
RETURN 
SUMX(Orders, Revenue)
```

Here, the `Revenue` variable stores the result of `Orders[Quantity] * Orders[UnitPrice]`, making the formula easier to read and maintain.

### Example 2: Calculating Profit Margin

Imagine you want to calculate the profit margin for each order, where the profit is the difference between the total revenue and the cost, and the margin is calculated as a percentage:

```excel
Profit Margin = 
VAR Revenue = Orders[Quantity] * Orders[UnitPrice]
VAR Cost = Orders[Quantity] * Products[CostPrice]
VAR Profit = Revenue - Cost
RETURN 
DIVIDE(Profit, Revenue, 0)
```

In this example, we use three variables:

* `Revenue` to store the revenue calculation.
    
* `Cost` to store the cost calculation.
    
* `Profit` to calculate the difference between revenue and cost.
    

Finally, the `DIVIDE` function calculates the profit margin as a percentage, with the variables making the formula much easier to follow.

### Example 3: Applying Conditional Logic with Variables

Suppose you want to apply a discount based on the total amount of an order. If the total amount exceeds $1,000, apply a 10% discount; otherwise, apply a 5% discount:

```excel
Discounted Amount = 
VAR TotalAmount = Orders[Quantity] * Orders[UnitPrice]
VAR Discount = IF(TotalAmount > 1000, 0.10, 0.05)
RETURN 
TotalAmount * (1 - Discount)
```

Here, we define two variables:

* `TotalAmount` stores the total amount of the order.
    
* `Discount` stores the discount percentage based on the condition.
    

The final `RETURN` statement calculates the discounted amount using these variables.

### Example 4: Calculating Weighted Average Price Using Variables

If you want to calculate the weighted average price of products sold, where the weight is determined by the quantity sold, you can use variables to simplify the calculation:

```excel
Weighted Average Price = 
VAR TotalRevenue = SUMX(Orders, Orders[Quantity] * Orders[UnitPrice])
VAR TotalQuantity = SUM(Orders[Quantity])
RETURN 
DIVIDE(TotalRevenue, TotalQuantity, 0)
```

In this example:

* `TotalRevenue` stores the sum of revenue across all orders.
    
* `TotalQuantity` stores the sum of quantities across all orders.
    

The `DIVIDE` function then calculates the weighted average price using these variables.

### Example 5: Optimizing Complex Measures

Let’s say you have a complex measure that needs to calculate the total revenue, apply a tax rate, and then calculate the net revenue. By using variables, you can break down this complex measure into simpler parts:

```excel
Net Revenue = 
VAR TotalRevenue = SUMX(Orders, Orders[Quantity] * Orders[UnitPrice])
VAR TaxRate = 0.15
VAR TaxAmount = TotalRevenue * TaxRate
RETURN 
TotalRevenue - TaxAmount
```

Here, variables help to clearly separate each part of the calculation:

* `TotalRevenue` calculates the sum of all revenue.
    
* `TaxRate` defines the tax rate (15% in this case).
    
* `TaxAmount` calculates the total tax based on the revenue.
    

Finally, the net revenue is calculated by subtracting the tax amount from the total revenue.

## Conclusion

Using variables in Power BI DAX is a powerful technique to simplify complex calculations, improve readability, and enhance performance. By breaking down formulas into smaller, reusable parts, variables help make your DAX expressions more manageable and easier to debug.