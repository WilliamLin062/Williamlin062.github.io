---
title: SQLServer還原DB
date: 2023-06-13 08:56:30
tags: 
 - t-sql
 - SQL
 - SQLServer
---
# 還原DB

### 

```sql
--讀取備份檔案，因為要知道到Data跟Log的名稱 這邊的N代表是Unicode
RESTORE FILELISTONLY
FROM DISK = N'[your.bak PATH]'
go
--要先創建一個新資料庫還有記得切換到master，還原到正在執行時的資料庫時記得確定沒有東西在使用資料庫不然會跳錯
restore database [newDB NAME]
from DISK = N'[your.bak PATH]'
WITH  
move '[DataFileName]' to '[newDB_DataFileName.mdf_PATH]',
move '[LogFileName]' to '[newDB_LogFileName.ldf_PATH]',
Replace, Recovery
GO

```
