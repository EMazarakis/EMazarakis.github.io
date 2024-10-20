---
layout: post
title: "Unlocking the Power of Partitioning in Power BI: The Game-Changer for Semantic Models!" 
date: 2024-10-19
categories: blog
author: "Eugene Mazarakis"
tags: [PBi Desktop, PBi Service, Incremental Refresh, Semantic Model]
---

# Introduction 
Power BI offers the capability, when designing a semantic model containing tables, to create partitions within these tables, enabling faster loading of large tables into the model. This is achieved by refreshing only the data of the most recent partitions during each refresh cycle. This functionality is particularly useful, as it significantly reduces the time required to refresh the semantic model in Power BI service, and also allows for the creation of models that include extremely large tables.

Let's assume that we have the following architecture:

![Photo 0](/assets/Img/BlogImages/004.BlogPost_19_10_2024/000.architecture.jpg)

Our data resides in Azure Synapse Analytics, and we will connect to it using Power BI Desktop to create a semantic model. We will then apply an incremental refresh policy, which enables the creation of partitions in the tables. Once the model is ready, we will publish it to Power BI Service within a premium workspace.

>  **NOTE**
> While it is possible to publish the model in a shared workspace, this option comes with certain limitations regarding the management of the model’s partitions. [See Section: Shared Capacity Limitations](#shared-capacity-limitations)


## Semantic model specifications
The semantic model to be created will have the following specifications:
1. Import storage mode will be utilized.
   - Partitioning works only with this type of mode.
3. Two parameters will be implemented to enable incremental refresh. [See References 3,4](#references)
   - RangeStart & RangeEnd (be careful **case sensitive** and **DateTime** data type)
4. Incremental refresh policy will be applied. [See References 1,2](#references)
   - Partitions will be created for **XXX historical** months and **YYY refresh period** months. For this case, we will work with the month option.
   - Alternatively, we can select years, quarters, or days for the historical period and refresh period.

## Steps for Model Creation
In order to create and publish the semantic model, we do the following steps:
1. Open a new Power BI (.pbix) file.
2. Select Get Data > Azure > Azure Synapse Analytics SQL, then click Connect.
3. In the pop-up window, enter the following details:
   - Server
   - Database
   - Import: Enabled
   - SQL statement: Include the appropriate SQL query here if you prefer not to load the entire table.
4. In the next window, you'll see a data preview. Click Load.




>  **TIP**

## Shared Capacity Limitations
1. A shared capacity workspace has a semantic model/report size limit of 1 GB.
2. We cannot connect to a Power BI shared workspace using SSMS or Tabular Editor to manipulate the tables’ partitions. [See References 8,9,11](#references)


## References
1. [Incremental Refresh Overview](https://learn.microsoft.com/en-us/power-bi/connect-data/incremental-refresh-overview)
2. [Define Incremental Refresh Policy on table](https://learn.microsoft.com/en-us/power-bi/connect-data/incremental-refresh-configure#define-policy)
3. [Create Parameters](https://learn.microsoft.com/en-us/power-bi/connect-data/desktop-dynamic-m-query-parameters#create-and-use-dynamic-parameters)
4. [Reference Parameters in the M query](https://learn.microsoft.com/en-us/power-bi/connect-data/desktop-dynamic-m-query-parameters#reference-the-parameters-in-the-m-query)
5. [Partitions](https://learn.microsoft.com/en-us/power-bi/connect-data/incremental-refresh-xmla#partitions)
6. [Refresh History of the semantic model](https://learn.microsoft.com/en-us/power-bi/enterprise/service-premium-connect-tools#semantic-model-refresh)
7. [size of the Semantic Model on PBI Service](https://community.fabric.microsoft.com/t5/Service/How-do-I-check-the-size-of-a-dataset-published-to-to-the-Power/m-p/1054316#M94213)
8. [Get the workspace connection URL](https://learn.microsoft.com/en-us/power-bi/enterprise/service-premium-connect-tools#to-get-the-workspace-connection-url)
9. [Connect to workspace by using SSMS](https://learn.microsoft.com/en-us/power-bi/enterprise/service-premium-connect-tools#connect-to-a-workspace-by-using-ssms)
10. [Refresh Partition with SSMS](https://learn.microsoft.com/en-us/power-bi/connect-data/incremental-refresh-xmla#refresh-management-with-sql-server-management-studio)
11. [Tabular Editor Apply Incremental Refresh Policy](https://learn.microsoft.com/en-us/power-bi/connect-data/incremental-refresh-xmla#apply-refresh-policy) 
