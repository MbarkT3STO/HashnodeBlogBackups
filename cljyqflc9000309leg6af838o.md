---
title: "MongoDB: How to Install?"
datePublished: Tue Jul 11 2023 20:15:57 GMT+0000 (Coordinated Universal Time)
cuid: cljyqflc9000309leg6af838o
slug: mongodb-how-to-install
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689103166082/838d01b7-15bc-472c-8b73-c28ab80077c1.webp
tags: nosql, mongodb, databases

---

MongoDB is a popular and powerful NoSQL database that offers high performance, scalability, and flexibility. If you're a Windows user and looking to install MongoDB on your machine, this blog post will guide you through the installation process step by step. Let's get started!

## **Prerequisites**

Before we begin, make sure you have the following prerequisites in place:

1. **Windows OS**: MongoDB supports Windows 7 or later versions, including Windows Server 2008 R2 or later.
    
2. **Administrator Access**: Ensure that you have administrative privileges on your Windows machine to install MongoDB.
    

## **Step 1: Download MongoDB Installer**

To install MongoDB, you need to download the MongoDB Community Edition installer. Follow these steps:

1. Visit the MongoDB website at [**https://www.mongodb.com/**](https://www.mongodb.com/) in your web browser.
    
2. Hover on "Products" in the top navigation menu then click on the "Comunity Server".
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689103327595/dd1fa0e0-d4ab-49a3-8fb3-70d3b25c8488.png align="center")

1. Scroll down and click on the "Select package" button.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689103505633/14657ced-5314-4a2e-a55a-b3ae989a23a5.png align="center")
    
2. Scroll a bit down and click on the "Download" button for the latest stable version compatible with Windows.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689103604346/867ca6f6-4815-4fc7-a235-c98363326168.png align="center")

1. Save the installer file to a location on your computer.
    

## **Step 2: Run the Installer**

Once the installer is downloaded, you can proceed with the installation process:

1. Locate the downloaded installer file and double-click on it to run the installer.
    
2. The MongoDB Setup Wizard will open. Click "Next" to continue.
    
3. Read and accept the MongoDB license agreement, then click "Next."
    
4. Choose the setup type. For most users, the "Complete" setup type is recommended, as it installs both the MongoDB server and tools. Click "Next" to proceed.
    
5. On the next screen, choose "Run service as network service user" option and click "Next".
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689105253291/9794aca6-eef5-49a9-b0df-44fba890dee1.png align="center")
    
    *You may change the default service name but it is recommended to keep the default "MongoDB" name to identify it easily.*
    
6. On the next screen, you can select additional features to install, such as MongoDB Compass, which provides a graphical interface for MongoDB. Make your selections and click "Next."
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689105397889/98408e10-8b60-409b-9e31-a8bb890982ae.png align="center")

1. Finally, click "Install" to start the installation process.
    

## **Step 3: Configure MongoDB**

After the installation is complete, you'll need to configure MongoDB:

1. On the final screen of the installer, leave the "Run MongoDB Compass" checkbox checked if you installed MongoDB Compass and want to launch it after installation. Click "Finish" to exit the installer.
    
2. Next, you'll need to add the MongoDB binaries to your system's PATH environment variable:
    
    * Open the Windows Start menu and search for "Environment Variables."
        
    * Select "Edit the system environment variables."
        
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689106014556/17011a5f-7718-47de-8f53-20c6b734d2a2.png align="center")
    
    * In the "System Properties" window, click on the "Environment Variables" button.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689106057722/3cd9356d-4840-42ee-bf00-bff313058e0e.png align="center")
        
    * In the "System variables" section, select the "Path" variable and click on the "Edit" button.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689106144628/4bfa2517-4f51-4676-aaee-e0470ea9764a.png align="center")
        
    * Click on the "New" button and add the path to the `bin` directory of your MongoDB installation. For example, `C:\Program Files\MongoDB\Server\{version_number}\bin` (*for me* `C:\Program Files\MongoDB\Server\6.0\bin`).
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689106252451/9956e913-97ca-4d5d-ae4f-45b93b472a47.png align="center")
        
    * Click "OK" to save the changes.
        
3. MongoDB is now installed and configured on your Windows machine.
    

### **Why Add the MongoDB Binaries to the PATH Environment Variable?**

Adding the MongoDB binaries to the system's PATH environment variable allows you to access MongoDB from any location in the command prompt without specifying the full path to the MongoDB executable. It enables you to run MongoDB commands and utilities from any directory without changing the current directory to the MongoDB installation folder.

## **Step 4: Verify the Installation**

To ensure that MongoDB is successfully installed, you can verify it using the command prompt:

1. Open the command prompt by pressing `Win + R` and typing `cmd`, then hit Enter.
    
2. In the command prompt, type `mongo --version` or `mongod --version` and press Enter.
    
3. If MongoDB is correctly installed, it will display the version number and other details of the installed MongoDB instance.
    

Congratulations! You have successfully installed MongoDB on your Windows machine. You can now start using MongoDB for your development projects and explore its powerful features.