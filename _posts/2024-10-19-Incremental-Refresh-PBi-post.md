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
- Open a new Power BI (.pbix) file.
- Select Get Data > Azure > Azure Synapse Analytics SQL, then click Connect.
   
![Photo 1](/assets/Img/BlogImages/004.BlogPost_19_10_2024/001.Get_Data.png) 


- In the pop-up window, enter the following details:
   - Server
   - Database
   - Import: Enabled
   - SQL statement: Include the appropriate SQL query here if you prefer not to load the entire table.

![Photo 2](/assets/Img/BlogImages/004.BlogPost_19_10_2024/002.Connect_to_synapse.png) 


- In the next window, you'll see data preview. Click Load.
   
![Photo 3](/assets/Img/BlogImages/004.BlogPost_19_10_2024/003.load_data_preview.png)


- After loading, a table will be created in Power BI model, typically named something like "Query1".

![Photo 4](/assets/Img/BlogImages/004.BlogPost_19_10_2024/004.Query_table_created.png) 


- Open the Power Query Editor to transform your "Query1" (e.g., renaming, adding parameters).
   - Go to Home > Transform > Transform Data.

![Photo 5](/assets/Img/BlogImages/004.BlogPost_19_10_2024/005.Transform_data.png) 


- In the Power Query Editor:
   - First, rename the "Query1" table to something more descriptive SALES_DATA (based on the business data).
   - Double-click on the table name to rename it.

![Photo 6](/assets/Img/BlogImages/004.BlogPost_19_10_2024/006.power_query_rename_table.png)
   
          
- Select the SALES_DATA table, then check the APPLIED STEPS pane on the right.
- To edit the SQL query (applicable only if you added a SQL statement in step 3).
    - Right-click the Source step and select Edit Settings or click the gear icon.
      
 ![Photo 7](/assets/Img/BlogImages/004.BlogPost_19_10_2024/007.applied_steps.png)

      
- You can now create the two parameters that will be used to implement the incremental refresh policy.
    - You are still inside the Power Query Editor.
    - Go to Home > Manage Parameters > New Parameter.
    - In the pop-up window, create the following two parameters:
         - RangeStart (Start Date for Incremental Refresh)
              - Required: Enabled
              - Type: Date/Time
              - Suggested Values: Any value
              - Current Value: Set a specific date
         - RangeEnd (End Date for Incremental Refresh)
             - Follow the same steps as for RangeStart.
             - Click OK.
                
![Photo 8](/assets/Img/BlogImages/004.BlogPost_19_10_2024/008.new_parameters.png)

![Photo 9](/assets/Img/BlogImages/004.BlogPost_19_10_2024/009.rangeStart.png)

![Photo 10](/assets/Img/BlogImages/004.BlogPost_19_10_2024/010.RangeEnd.png)    
 
         
- The two newly created parameters will now appear in the Queries pane of the Power Query Editor.

![Photo 11](/assets/Img/BlogImages/004.BlogPost_19_10_2024/011.params.png)


- Update the SQL query to include the parameters for filtering the data:
    - Select the SALES_DATA table, right-click, and choose Advanced Editor.
      
![Photo 12](/assets/Img/BlogImages/004.BlogPost_19_10_2024/012.advanced_editor.png)


- In the M query editor, modify the SQL query by adding M query functions, two times.

![Photo 13](/assets/Img/BlogImages/004.BlogPost_19_10_2024/013.M_query_editor.png)


- Go to the WHERE clause in the SQL query (where the date filters are).
- Modify the expression by adding the following lines:
    - This line of code replace the value of the date on >= side: & DateTime.ToText(RangeStart, "yyyy-MM-dd") &
    - This line of code replace the value of the date on < side: & DateTime.ToText(RangeEnd, "yyyy-MM-dd") &

![Photo 14](/assets/Img/BlogImages/004.BlogPost_19_10_2024/014.add_expressions.png)


- A warning message may appear in the Power Query Editor for the modified table. Click Edit Permissions.

![Photo 15](/assets/Img/BlogImages/004.BlogPost_19_10_2024/015.edit_permission_message.png)


20. A pop-up window will display the transformed query with the parameter values applied. Click Run to load the filtered data.
21. Once done, click Close & Apply from the Home menu to apply the changes.
22. Now, you are out of the power query editor. It's time to set-up the incremental refresh policy for the SALES_DATA table.
    - Select the SALES_DATA table from the Data pane, go to More options (...) > Incremental Refresh.
23. In the pop-up window, do the followings:
    - First, select the desired table.
    - Enable the option Set import and refresh ranges.
    - Define the historical period for partitioning and refresh:
      - Archive data starting [X months] before refresh date (e.g., 3 months).
      - Incrementally refresh data starting [X months] before refresh date (e.g., 1 month, to define the last level of partition).
    - Click Apply.
24. The incremental refresh policy is now defined but not yet applied.
25. Publish the model to Power BI Service:
    - Go to Home > Publish, and select the desired workspace.
26. After publishing, you will see two items in the Power BI Service workspace:
   - The Semantic Model.
   - The Report. (This should be empty.)
24. Set up the connection between Power BI Service and the Synapse source (handled by the service admin/owner):
   - Go to the semantic model in Power BI Service, select More options (...) > Settings.
   - Under Gateway and Cloud options, ensure the connection settings are correct.
25. Perform an on-demand refresh:
   - In the Power BI Service workspace, select the model and click Refresh Now.
   - During this step, the incremental refresh policy will be applied, loading data and creating partitions. This may take a while.
26. To see the refresh duration:
   -	Go to the semantic model, click More options (...) > Refresh History.




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
