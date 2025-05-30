---
layout: post
title: "Index of Fabric Workspace Items"
date: 2025-05-18
categories: blog
author: "Eugene Mazarakis"
tags: [Fabric, Notebook, Workspace, Lakehouse, Power BI]
---

## Introduction
Often in Fabric, you need to locate a specific report or item (such as a notebook) but can’t remember which workspace it’s in. Typically, this involves using Fabric’s search bar or manually checking each workspace one by one. But what if you want to perform a more specific search?

Now, you can query the related table stored in the Lakehouse, which contains all the relevant information for each Fabric workspace. Alternatively, you can use a Power BI report that provides the same data and allows you to search directly and quickly identify the workspace containing the item you’re looking for.

**The lakehouse table**

![Photo 0](/assets/Img/BlogImages/009.BlogPost_18_05_2025/table.PNG)


### Steps
1. Create an empty lakehouse.
2. Create a notebook with the related python code.
3. Query the table in lakehouse through the SQL endpoint.
4. Then you can go to create a power bi report with a search bar.


Following the python code:
```
#The following Python code creates a table in the lakehouse, listing the items contained in each workspace.
import pandas as pd
import sempy.fabric as fabric
 
# List workspaces and capacities
workspaces = fabric.list_workspaces()
capacities = fabric.list_capacities()

 
# Convert to pandas DataFrames
workspaces_df = pd.DataFrame(workspaces)
capacities_df = pd.DataFrame(capacities)


# Join workspaces with capacities on the capacity ID
workspaces_with_capacity = workspaces_df.merge(capacities_df, left_on='Capacity Id', right_on='Id', suffixes=('_workspace', '_capacity'))


# Filter the workspaces that are on dedicated capacity
workspaces_on_dedicated_capacity = workspaces_with_capacity.query('`Is On Dedicated Capacity` == True') 


# Select relevant columns
result_tmp = workspaces_on_dedicated_capacity[['Name', 'Id_workspace' ,'Display Name', 'Capacity Id', 'Sku', 'Region', 'State']].rename(columns={'Name': 'Workspace_Name', 'Id_workspace': 'Workspace_Id','Display Name': 'Capacity_Name', 'Capacity Id':'Capacity_Id'})


# Create a list to hold all items from the workspaces
all_items_dfs = []

# Loop through workspaces and gather items
for ws in result_tmp['Workspace_Name'].values:
    items = fabric.list_items(workspace=ws)
    items_df = pd.DataFrame(items)
    items_df['Workspace_Name'] = ws             # Add workspace name to each row
    all_items_dfs.append(items_df)

# Concatenate all_items_dfs into one Dataframe
all_items_combined_df = pd.concat(all_items_dfs, ignore_index=True)

# Rename all the columns
all_items_combined_df = all_items_combined_df.rename(columns={'Display Name': 'Item_Name', 'Description': 'Item_Description', 'Type':'Item_Type', 'Workspace_Name': 'Name_of_Workspace','Workspace Id': 'Id_of_Workspace'})


# Join workspaces and capacities with Workspace_Items on the Workspace Id
workspaces_with_items = result_tmp.merge(all_items_combined_df, left_on='Workspace_Id', right_on='Id_of_Workspace')


# Select relevant columns
result = workspaces_with_items[['Capacity_Name','Capacity_Id','Sku','Region','State','Workspace_Name','Name_of_Workspace','Workspace_Id','Id_of_Workspace','Item_Name','Item_Description','Item_Type']]


# Display the result in a tabular format
display(result.sort_values(by='Name_of_Workspace', ascending=True))


# Let's save dataframe as a delta lake, parquet table to Tables section of the default lakehouse
delta_table_name = 'workspaces_items'
spark.createDataFrame(result).write.mode("overwrite").format("delta").saveAsTable(delta_table_name)

```

**Lakehouse Table**

Now that the table has been created, you can run a query to search for any item in the Fabric workspaces using the LIKE operator with wildcards.
![Photo 1](/assets/Img/BlogImages/009.BlogPost_18_05_2025/query_list_tables.PNG)

---

**Power BI Report**

The following is the Power BI report. Using the search bar, you can search for any item within the list visualization.
![Photo 2](/assets/Img/BlogImages/009.BlogPost_18_05_2025/power_bi_report_searching.PNG)
