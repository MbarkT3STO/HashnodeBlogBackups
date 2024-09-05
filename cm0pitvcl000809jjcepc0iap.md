---
title: "Power BI: Decomposition Tree Visual"
datePublished: Thu Sep 05 2024 16:48:11 GMT+0000 (Coordinated Universal Time)
cuid: cm0pitvcl000809jjcepc0iap
slug: power-bi-decomposition-tree-visual
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1725553899905/87ee0d57-966f-4cfc-892c-e4bd3d1b3b13.avif
tags: data-science, data-analysis, business-intelligence, powerbi

---

In this blog post, we’ll explore the Decomposition Tree visual in Power BI, show how to create it, and provide practical examples using a sales dataset. The examples will include actual results to demonstrate the visual’s full potential.

## What is a Decomposition Tree Visual?

The Decomposition Tree visual is an AI-powered feature in Power BI that allows users to analyze and break down a single measure by one or more dimensions. It can be used to explore key drivers and break down totals, such as sales or profits, into smaller contributing factors.

### When to Use a Decomposition Tree?

* **Root cause analysis**: Understand the factors driving a specific outcome.
    
* **Performance analysis**: Break down sales, profits, or other key metrics by various dimensions such as product category, region, and time.
    
* **Exploration**: Use Power BI’s AI capabilities to automatically find the next best factor in the hierarchy for analysis.
    

## **Dataset Overview**

Let’s briefly review the dataset ([**Download it from here**)](https://y15w7-my.sharepoint.com/:x:/g/personal/me_mbvrk_onmicrosoft_com/EQ3lhi9e9LRKmRs4kwMFT8cBT4OYU79rqIT5rxFcBk4rrA?e=ncueOT) we’ll use for our examples. The dataset contains the following tables:

* **Customers**: Contains customer information such as ID, name, gender, country, city, age, and marital status.
    
* **Products**: Holds product details including title, category, unit price, weight, and manufacturer.
    
* **Stocks**: Displays current stock levels for each product.
    
* **Orders**: Captures order transactions including customer ID, product ID, quantity, unit price, and total amount.
    

## How to Create a Decomposition Tree in Power BI

### Step 1: Load Your Data

1. **Open Power BI Desktop.**
    
2. **Load your Excel file** containing the sales dataset.
    
3. Ensure that relationships between tables (e.g., `Orders` and `Products`) are properly set.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1725469462497/1776e8f2-e2ca-432f-b862-5c14d9dda7c9.png?auto=compress,format&format=webp align="left")
    

### Step 2: Add the Decomposition Tree Visual

1. In the **Report view**, select the **Decomposition Tree visual** from the Visualizations pane.
    
2. You’ll see two fields: **Analyze** and **Explain By**.
    
    * **Analyze**: The field where you place the measure to be analyzed (e.g., total revenue or profit).
        
    * **Explain By**: The field where you place the dimensions you want to break the measure down by (e.g., product category, country, age group).
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1725554272407/940c2e4f-0a0c-403e-8a07-6a49ffd10ab7.png align="center")
        

## Practical Examples

### Example 1: Analyzing Total Revenue by Product Category

Let’s create a Decomposition Tree to break down total revenue by product category and country.

1. **Analyze**: Drag `Orders[TotalAmount]` into the Analyze field.
    
2. **Explain By**: Drag `Products[Category]` and `Customers[Country]` into the Explain By field.
    

#### Resulting Decomposition Tree:

* The Decomposition Tree will first display the total revenue.
    
* As you expand the tree, you can break down revenue by product category.
    
* After expanding by product category, you can further break down revenue by country.
    

For instance:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1725554508509/06010196-eadd-4164-820a-f274bca27b20.png align="center")

This example illustrates how to explore revenue distribution across both product categories and geographic regions.

### Example 2: Exploring Sales Quantity by Customer Gender and Age

Suppose you want to analyze how sales quantity varies by customer gender and age.

1. **Analyze**: Drag `Orders[Quantity]` into the Analyze field.
    
2. **Explain By**: Drag `Customers[Gender]` and `Customers[Age]` into the Explain By field.
    

#### Resulting Decomposition Tree:

* The top node will display the total quantity of products sold.
    
* Expanding the tree by gender will show how much men and women purchased.
    
* You can further expand each gender node by age to see sales distribution across different age segments.
    

For example:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1725554669712/6e10ef0f-2f82-4994-8e55-31436ab34d8c.png align="center")

This example helps identify which age are purchasing the most products and highlights differences between genders.

## Using AI Splits in the Decomposition Tree

One of the most powerful features of the Decomposition Tree visual is the ability to use AI splits to automatically find the next best dimension for analysis.

* When expanding a node, you’ll notice the **lightbulb icon**. Clicking on this icon allows Power BI to automatically find the dimension that contributes the most to the measure you are analyzing.
    
* This feature is great for discovering hidden patterns in your data that may not be immediately obvious.
    

## Tips for Using the Decomposition Tree Visual

* **Start with Key Metrics**: Focus on a key measure such as total revenue, profit, or quantity sold.
    
* **Use AI Insights**: Leverage Power BI’s AI capabilities to automatically surface key drivers that you might not have considered.
    
* **Combine Multiple Dimensions**: Use multiple dimensions like product category, country, and gender to get a deeper understanding of your data.
    

## Conclusion

The Decomposition Tree visual in Power BI is a powerful tool for exploring data hierarchies and conducting in-depth analysis. Whether you’re investigating revenue drivers, analyzing sales patterns by customer demographics, or identifying profit trends, the Decomposition Tree enables you to break down data step by step.