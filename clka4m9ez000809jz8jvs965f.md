---
title: "PostgreSQL: Install"
datePublished: Wed Jul 19 2023 19:38:31 GMT+0000 (Coordinated Universal Time)
cuid: clka4m9ez000809jz8jvs965f
slug: postgresql-install
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689587360172/be06e38c-49ef-43fa-849d-0bc1f71f6f35.png
tags: postgresql, nosql, databases, sql

---

8 PostgreSQL, often referred to as Postgres, is a powerful open-source relational database management system known for its robust features, scalability, and extensibility. Whether you are a developer, data analyst, or database administrator, installing PostgreSQL on your system is the first step to leverage its capabilities. In this guide, we will walk you through the installation process on various platforms with easy-to-follow examples.

## Prerequisites

Before we start the installation, ensure that you have the following prerequisites in place:

1. **Operating System**: PostgreSQL supports a wide range of operating systems, including Windows, macOS, and Linux. Make sure you have a compatible operating system version installed on your machine.
    
2. **System Requirements**: PostgreSQL has specific system requirements based on your intended use and workload. Review the official PostgreSQL documentation for detailed information on system requirements: [PostgreSQL Documentation](https://www.postgresql.org/docs/current/install-requirements.html)
    
3. **Administrative Privileges**: For some installations, you might need administrative privileges (root or superuser access) on your system.
    

## Installation Steps

### Step 1: Windows Installation

1. Download the PostgreSQL Installer for Windows from the official website: [PostgreSQL Downloads](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads)
    
2. Run the installer and follow the on-screen instructions.
    
3. During the installation process, you will be prompted to choose the components to install. Make sure to select the "pgAdmin" tool if you want a graphical administration interface.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689587481835/62f967e4-dfbc-41d5-b35b-91246549945d.png align="center")
    
4. Choose a password for the database superuser (postgres) when prompted. Remember this password as you will need it later.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689587543708/7fde79e9-8e7d-4660-9bae-c0c6f4051f72.png align="center")
    
5. Choose the port
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689587676745/0bfc1a77-ab5c-4ac6-80c5-0121d1edd3bb.png align="center")
    
6. Complete the installation process, and PostgreSQL will be installed on your Windows machine.
    

### Step 2: macOS Installation

1. PostgreSQL can be installed on macOS using Homebrew, a popular package manager for macOS. Open Terminal and run the following command:
    
    ```bash
    brew install postgresql
    ```
    
2. Homebrew will handle the installation, and once the process is complete, PostgreSQL will be ready to use on your macOS system.
    

### Step 3: Linux Installation

1. PostgreSQL is available in most Linux distribution repositories. To install it, open a terminal and use the package manager specific to your distribution:
    
    For Ubuntu/Debian:
    
    ```bash
    sudo apt-get update
    sudo apt-get install postgresql
    ```
    
    For Fedora:
    
    ```bash
    sudo dnf install postgresql-server
    ```
    
    For CentOS/RHEL:
    
    ```bash
    sudo yum install postgresql-server
    ```
    
2. Once the installation is finished, PostgreSQL will be running as a service on your Linux machine.
    

## Important

Now you can start the `PgAdmin` to control the the server/databases, but sometime on windows the default installed `PgAdmin` won't start correctly, to fix this issue just download it as a separate version from [this link](https://www.pgadmin.org/download/pgadmin-4-windows/), then install it aaaaaand yes the issue will be fixed.