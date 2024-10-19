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
> While it is possible to publish the model in a shared workspace, this option comes with certain limitations regarding the management of the model’s partitions. [Go to Section: Shared Capacity Limitations](#Shared Capacity Limitations)


## Semantic model specifications
The semantic model to be created will have the following specifications:
1. Import storage mode will be utilized.
   - Partitioning works only with this type of mode.
3. Two parameters will be implemented to enable incremental refresh.
   - RangeStart & RangeEnd (be careful **case sensitive** and **DateTime** data type)
4. Incremental refresh policy will be applied:
   - Partitions will be created for **XXX historical** months and **YYY refresh period** months. For this case, we will work with the month option.
   - Alternatively, we can select years, quarters, or days for the historical period and refresh period.




>  **TIP**

## Shared Capacity Limitations
1. A shared capacity workspace has a semantic model/report size limit of 1 GB.
2. We cannot connect to a Power BI shared workspace using SSMS or Tabular Editor to manipulate the tables’ partitions. (TODO)
