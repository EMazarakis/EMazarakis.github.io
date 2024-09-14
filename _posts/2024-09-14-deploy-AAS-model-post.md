---
layout: post
title: "How to deploy from UAT env to Production env an AAS tabular model"
date: 2024-09-14
categories: blog
author: "Eugene Mazarakis"
tags: [Azure, AAS, Visual Studio, Tabular Model]
---

Set up the deployment process step by step.
## Introduction

Tabular models in Analysis Services are databases that run in-memory or in Direct Query mode, connecting to data from back-end relational data sources. Tabular models in Analysis Services are databases that run either in-memory or in Direct Query mode, connecting to data from back-end relational data sources. Using Microsoft Visual Studio, the authoring tool, we can create tabular models. The data model provides an easier and faster way for users to perform ad hoc data analysis with tools like Power BI and Excel.

### Photo 0

## Edit the Model.bim file
If we have a tabular model on UAT environment and want to transfer it to Production environment we should edit the Model.bim file and do the following steps:
1. Inside the Visual Studio project, we go to Tabular Model Explorer. Here we can see the name of the data source.

### Photo 1

2. Click on the Solution Explorer tab.

### Photo 2

3. Right-click on the Model.bim file and select View Code.
(Italics)The Model.bim file contains the metadata for the model project, which is why we need to make changes there.

### Photo 3

4. When the Model.bim file is open, you will see the data source at the top.
5. Replace the existing data source name with the desired new name.
(Italics) You can choose a general name, e.g., SOURCE_AZURE_SYNAPSE_NAME.
6. Next, update the server and database in the address JSON label with the Production environment values.
7. Also, update the path and username in the credentials JSON label with the Production environment values.

### Photo 4

8. Next, navigate to each table in the partitions JSON label.
9. In the expression JSON label, enter the data source name from step 5 in the let → source field.

### Photo 5

10. Save the changes to the Model.bim file.
(Italics) If the Tabular Model Explorer disappears at any time, simply double-click the Model.bim file to reopen it.
11. When you go to the Tabular Model Explorer, you should now see the new data source name you selected in step 5 under Data Sources.

### Photo 6

12. Finally, we need to Process Table for each table in the model.
13. Now, we are ready to deploy the model to the production server. In Solution Explorer, right-click on the project name and go to Properties. Update the deployment server with the Production environment values.

### Photo 7
### Photo 8
### Photo 9


## Wrap-Up
And that’s it—a model successfully deployed to the production environment. We only needed to make changes in one location within the Model.bim file, which contains all the necessary metadata for the tabular project.
I hope you found this article useful. Thank you for reading!
