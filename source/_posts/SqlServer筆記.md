---
title: SqlServer筆記
date: 2023-06-20 14:55:50
tags: 
    - SQlServer
---
# 動機

### 紀錄遇到的問題和解決方法，加深記憶

# 使用TempTable

##### 使用select into [table_name]會建立新的暫存表，使用前要確認暫存表還在不在，在的話要記得刪掉不然會錯，使用完要記得刪掉

```sql
DROP TABLE IF EXISTS #[table-name]

select column1, column2, column3, ... as [name]
into #[tablename]
from [table] [簡稱]

DROP TABLE IF EXISTS #[table-name]
```

# 合併沒共通鍵的表

### 使用cross join或者join on 1=1都可以，建議使用cross join語意上會直觀很多

```sql
select * from #temp1 CROSS JOIN #temp2,#temp3
```
