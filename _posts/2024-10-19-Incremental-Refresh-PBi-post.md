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

