---
title: "MongoDB: Shell"
datePublished: Wed Jul 12 2023 12:03:55 GMT+0000 (Coordinated Universal Time)
cuid: cljzoaoij000009ladvew08v7
slug: mongodb-shell
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689162453415/7fc19363-c119-4344-839d-6bcffb0d7bde.webp
tags: nosql, mongodb, databases

---

MongoDB is a popular open-source NoSQL database that offers high performance, scalability, and flexibility for building modern applications. To interact with MongoDB and perform administrative tasks, developers often use the MongoDB Shell, a powerful command-line tool. In this blog post, we will guide you through the process of installing the MongoDB Shell on Windows, step by step.

## **Prerequisites**

Before installing the MongoDB Shell, make sure you have the following prerequisites:

1. Windows operating system (compatible versions include Windows 7, Windows Server 2008 R2, or later)
    
2. Administrative access to your machine
    

## **Step 1: Download the MongoDB Shell Installer**

To begin the installation process, follow these steps:

1. Open a web browser and navigate to the MongoDB download page: [**https://www.mongodb.com/try/download/shell**](https://www.mongodb.com/try/download/shell).
    
2. Select "Windows 64-bit (8.1+) (MSI)" option
    
3. Scroll a bit down to the "Download" button then click on it.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689162901716/e6a3f19f-bdbe-46fc-a23a-2ecb8ee8f537.png align="center")

## **Step 2: Run the Installer**

Once the MongoDB Shell installer is downloaded, follow these steps to run it:

1. Locate the downloaded installer file, which typically resides in your system's default download location.
    
2. Double-click on the installer file to launch the MongoDB Shell Setup Wizard.
    
3. The wizard will guide you through the installation process. Click "Next" to proceed.
    
4. Choose the installation location. By default, it will be installed in the `C:\Program Files\MongoDB\Server\<version>` directory (*for me it is* `C:\Program Files\MongoDB\Server\6.0`). You can change the location if desired.
    
5. Click "Install" to begin the installation process.
    

## **Step 3: Verify the Installation**

After the installation is complete, it's important to verify that the MongoDB Shell is working correctly. Here's how you can do it:

1. Open the Windows Command Prompt by pressing `Win + R` and typing "cmd" followed by pressing Enter.
    
2. In the Command Prompt, type `mongo` or `mongosh` and press Enter.
    
3. If the MongoDB Shell successfully launches, you will get a screen as the following
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689163349776/2b034ee9-beac-4931-bf62-4ef82a9523c3.png align="center")
    
4. You can now start interacting with the MongoDB Shell and execute various commands.