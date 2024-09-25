---
layout: post
title: "ISO Year on MS SQL database"
date: 2024-09-25
categories: blog
author: "Eugene Mazarakis"
tags: [ISOYear, MS SQL db, Azure Synapse Anlytics]
---


### Vertica
```
SELECT a.testDate, YEAR(a.testDate), YEAR_ISO(a.testDate)
FROM
(
	select DATE('2024-12-31') as testDate
	UNION
	select DATE('2023-12-31')
	UNION
	select DATE('2022-12-31')
	UNION
	select DATE('2021-12-31')
	UNION
	select DATE('2020-12-31')
	UNION
	select DATE('2019-12-31')
	UNION
	select DATE('2018-12-31')
)a
ORDER BY 1 DESC
```
