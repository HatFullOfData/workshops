+++
title = "Lab 2: Lakehouses and Warehouses"
weight = 20
+++

## Introduction

In this lab, you’ll create a lakehouse from the **Store data** task and a warehouse from the **Gold Layer** task in your Fabric task flow. A lakehouse can hold files and tables for different data workloads, while a warehouse focuses on structured data and SQL. You’ll explore each item, then compare them.

## Creating a Lakehouse

First, you are going to add a lakehouse to the **Store data** task.

> [!NOTE]Instructions
> 1. Open the workspace and view task flow you created in the previous lab.
> 1. On the **Store data** task, select **New item**.
> 1. Choose **Lakehouse**.
> 1. Give it a unique name that includes your initials, such as `LGB_Workshop_Lakehouse`. **Note** you can only use letter, numbers and underscores in the name.
> 1. Click **Create**

![Screenshots of the above instructions](<create lakehouse.png>)

### Exploring the Lakehouse

Take a few minutes to look around the lakehouse:

- In **Explorer**, look for the **Tables** and **Files** areas. Tables are for structured, queryable data; Files can hold data in its original file format.
- The lakehouse is empty until data is added. 
- In the Workspace you can see the Lakehouse and the SQL Endpoint. We will return to use the Endpoint once we have data to explore.

![Screenshot of the Explorer pane and the Lakehouse in the workspace](<explore lakehouse.png>)

## OneLake and Delta

**OneLake** is Fabric’s shared data lake, where data from Fabric items is stored and can be used across workloads. **Delta Lake** is an open table format used for Fabric tables; it adds structure and transaction support to data stored in files. Together, they help different Fabric experiences work with the same data.

## A Simple Medallion Architecture

A medallion architecture organises data into stages: **Bronze** keeps data close to its original form, **Silver** contains cleaned and refined data, and **Gold** contains data prepared for reporting and analysis. Bronze, Silver, and Gold are common examples, not a required number of layers; a solution can use as many layers as its transformation and cleaning needs require. These are logical stages, not specific Fabric item types. In this workshop, we’ll use the lakehouse for the Bronze and Silver layers, then the warehouse for the final Gold layer. Keeping stages separate can make it easier to understand how data changes along the way.

## Creating a Warehouse in the Gold Layer

Next, add a warehouse to the **Gold Layer** task.

> [!NOTE]Instructions
> 1. Return to the task flow in the workspace.
> 1. On the **Gold Layer** task, select **New item**.
> 1. Choose **Warehouse**.
> 1. Give it a unique name that includes your initials, such as `LGB_Workshop_Warehouse`
> 1. Select Create to finish.

![Screenshots of the above instructions](<create warehouse.png>)

### Exploring the Warehouse

Open the warehouse and look at its Explorer pane. A warehouse is organised around schemas and tables, and its SQL query experience is designed for working with structured data. It will be empty until you add data.

![Explorer pane of the empty warehouse](<explore warehouse.png>)

## Lakehouse and Warehouse: What’s the Difference?

Both items store analytical data in OneLake and can be used with Power BI. The main difference is how you work with the data:

| Lakehouse | Warehouse |
|---|---|
| Combines files and tables, making it suitable for data engineering and working with data in different formats. | Organises structured data into tables and is designed for SQL-based data warehousing. |
| Commonly explored and transformed with Spark, notebooks, or other data engineering tools. | Commonly created, loaded, and queried with T-SQL. |
| Includes a SQL analytics endpoint for querying its tables. | Provides a SQL warehouse experience for managing and querying its data. |

## Useful References

- [Microsoft Learn - What is a lakehouse in Microsoft Fabric?](https://learn.microsoft.com/en-us/fabric/data-engineering/lakehouse-overview?wt.mc_id=DX-MVP-5003563)
- [Microsoft Learn - What is a warehouse in Microsoft Fabric?](https://learn.microsoft.com/en-us/fabric/data-warehouse/data-warehousing?wt.mc_id=DX-MVP-5003563)
- [Microsoft Learn - Warehouse and Lakehouse: A Decision Guide](https://learn.microsoft.com/en-us/fabric/fundamentals/decision-guide-lakehouse-warehouse?wt.mc_id=DX-MVP-5003563)
- [Microsoft Learn - OneLake, the unified data lake](https://learn.microsoft.com/en-us/fabric/onelake/onelake-overview?wt.mc_id=DX-MVP-5003563)
- [Microsoft Learn - Delta Lake overview](https://learn.microsoft.com/en-us/fabric/fundamentals/delta-lake-overview?wt.mc_id=DX-MVP-5003563)
