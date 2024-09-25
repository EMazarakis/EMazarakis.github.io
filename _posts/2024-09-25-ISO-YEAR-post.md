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

---

### Oracle

```
SELECT a.testDate, to_char(a.testDate, 'yyyy') as Year, to_char(a.testDate, 'iyyy') "ISO Year"
FROM
(
	select TO_DATE('2024/12/31', 'YYYY/MM/DD') as testDate from dual
	UNION
	select TO_DATE('2023/12/31', 'YYYY/MM/DD') from dual
	UNION
	select TO_DATE('2022/12/31', 'YYYY/MM/DD') from dual
	UNION
	select TO_DATE('2021/12/31', 'YYYY/MM/DD') from dual
	UNION
	select TO_DATE('2020/12/31', 'YYYY/MM/DD') from dual
	UNION
	select TO_DATE('2019/12/31', 'YYYY/MM/DD') from dual
	UNION
	select TO_DATE('2018/12/31', 'YYYY/MM/DD') from dual
)a
ORDER BY 1 DESC

```

---

### MS SQL db, Azure Synapse Analytics

```
SELECT	a.testDate
	, YEAR(a.testDate) AS YEAR
	, CASE 
		WHEN MONTH(a.testDate) = 1 AND DATEPART(ISO_WEEK,  a.testDate) > 51 THEN YEAR(a.testDate) - 1 
		WHEN MONTH(a.testDate) = 12 AND DATEPART(ISO_WEEK,  a.testDate) = 1  THEN YEAR(a.testDate) + 1 
		ELSE YEAR(a.testDate)
	END 
FROM
(
	select CAST('2024-12-31' AS DATE) as testDate
	UNION
	select CAST('2023-12-31' AS DATE)
	UNION
	select CAST('2022-12-31' AS DATE)
	UNION
	select CAST('2021-12-31' AS DATE)
	UNION
	select CAST('2020-12-31' AS DATE)
	UNION
	select CAST('2019-12-31' AS DATE)
	UNION
	select CAST('2018-12-31' AS DATE)
)a
ORDER BY 1 DESC
```

