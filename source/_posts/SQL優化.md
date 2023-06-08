---
title: SQL優化
date: 2023-06-06 17:50:53
tags: TSQL SQL SqlServer
---
# SQL Server事件監視器

### 開啟位置 [工具]->SQL Server Profiler

#### SQL Server Profiler是一個很有用的工具，可以看SQL在執行時實際在發生哪些事情

#### 還有查看TSQL在執行時的樣子，可以找出一些不好找到的錯誤還有追蹤效能


# SQL Server 執行計畫 用戶端統計資料

### 開啟位置 [查詢] -> 包括用戶端統計資料 / 包括實際執行計畫

#### 可以看到SQL執行每段查詢時的效能還有用戶端使用時間，可以幫助優化


# 用 UNION ALL 取代大量 OR

```sql
            --未修改前 執行時間為 12秒 CPU使用率100%
            DROP TABLE IF EXISTS #TempPreMemberJoin
            DROP TABLE IF EXISTS #TempPreMemberLeftJoin
            DROP TABLE IF EXISTS #TempPreMember
            DROP TABLE IF EXISTS #TempBlacklist
            DROP TABLE IF EXISTS #TempPreBlacklist

            --建立 PreMember暫存Table
            SELECT pmcu.* , cdcu.DEPT_Boss
            INTO #TempPreMember
            FROM (
            (SELECT cu.EP_DeptSno , cu.EP_CName , pm.*
             FROM CM_Pre_Member pm JOIN CP_User cu ON PM.EP_Id = cu.EP_Id) AS pmcu JOIN
            (SELECT DEPT_Sno , DEPT_Boss FROM CP_Dept cd ) AS cdcu ON pmcu.EP_DeptSno = cdcu.DEPT_Sno
            )

            --使用於Blacklist用
            SELECT *
            INTO #TempPreMemberJoin
            FROM (SELECT TPM.* FROM #TempPreMember TPM JOIN CP_User cu ON TPM.DEPT_Boss = cu.EP_Id) AS temp

            --使用於Select準會員
            SELECT *
            INTO #TempPreMemberLeftJoin
            FROM (SELECT TPM.* , cu.EP_Id AS BossID , cu.EP_CName AS BossName FROM #TempPreMember TPM LEFT JOIN CP_User cu ON TPM.DEPT_Boss = cu.EP_Id) AS temp


            --建立 Blacklist暫存Table
            SELECT AutoNo , CName ,
            CASE WHEN Tel1 IS NULL OR Tel1 = '' THEN NULL ELSE Replace(Tel1,'-','') END AS PM_Tel1 ,
            CASE WHEN Tel2 IS NULL OR Tel2 = '' THEN NULL ELSE Replace(Tel2,'-','') END AS PM_Tel2 ,
            CASE WHEN Tel3 IS NULL OR Tel3 = '' THEN NULL ELSE Replace(Tel3,'-','') END AS PM_Tel3
            INTO #TempBlacklist
            FROM ESIC_ERP.dbo.Blacklist WHERE Tel1 <> '' OR Tel2 <> '' OR Tel3 <> ''

-- 這段花費88%時間處理，一半時間在處理or巢狀迴圈另一半在處理 插入暫存表#TempPreBlacklist
            --建立BlackList內層 用於區分是否為黑名單成員
            --SELECT TB.AutoNo , TB.CName , TPMJ.PM_Sno , TPMJ.EP_CName
            SELECT TPMJ.* , TB.AutoNo , TB.CName , TB.PM_Tel1 AS T1 , TB.PM_Tel2 AS T2 , TB.PM_Tel3 AS T3
            INTO #TempPreBlacklist
            FROM #TempPreMemberJoin TPMJ JOIN #TempBlacklist TB
            ON TPMJ.EP_CName = TB.CName OR
            TPMJ.PM_Tel1 = TB.PM_Tel1 OR TPMJ.PM_Tel2 = TB.PM_Tel1 OR TPMJ.PM_Tel3 = TB.PM_Tel1 OR TPMJ.PM_Tel4 = TB.PM_Tel1 OR TPMJ.PM_Tel5 = TB.PM_Tel1 OR TPMJ.PM_Tel6 = TB.PM_Tel1 OR
            TPMJ.PM_Tel7 = TB.PM_Tel1 OR TPMJ.PM_Tel8 = TB.PM_Tel1 OR TPMJ.PM_Fax1 = TB.PM_Tel1 OR TPMJ.PM_Fax2 = TB.PM_Tel1 OR TPMJ.PM_Mobile1 = TB.PM_Tel1 OR TPMJ.PM_Mobile2 = TB.PM_Tel1 OR
            TPMJ.PM_Tel1 = TB.PM_Tel2 OR TPMJ.PM_Tel2 = TB.PM_Tel2 OR TPMJ.PM_Tel3 = TB.PM_Tel2 OR TPMJ.PM_Tel4 = TB.PM_Tel2 OR TPMJ.PM_Tel5 = TB.PM_Tel2 OR TPMJ.PM_Tel6 = TB.PM_Tel2 OR
            TPMJ.PM_Tel7 = TB.PM_Tel2 OR TPMJ.PM_Tel8 = TB.PM_Tel2 OR TPMJ.PM_Fax1 = TB.PM_Tel2 OR TPMJ.PM_Fax2 = TB.PM_Tel2 OR TPMJ.PM_Mobile1 = TB.PM_Tel2 OR TPMJ.PM_Mobile2 = TB.PM_Tel2 OR
            TPMJ.PM_Tel1 = TB.PM_Tel3 OR TPMJ.PM_Tel2 = TB.PM_Tel3 OR TPMJ.PM_Tel3 = TB.PM_Tel3 OR TPMJ.PM_Tel4 = TB.PM_Tel3 OR TPMJ.PM_Tel5 = TB.PM_Tel3 OR TPMJ.PM_Tel6 = TB.PM_Tel3 OR
            TPMJ.PM_Tel7 = TB.PM_Tel3 OR TPMJ.PM_Tel8 = TB.PM_Tel3 OR TPMJ.PM_Fax1 = TB.PM_Tel3 OR TPMJ.PM_Fax2 = TB.PM_Tel3 OR TPMJ.PM_Mobile1 = TB.PM_Tel3 OR TPMJ.PM_Mobile2 = TB.PM_Tel3

            --計算數量
            SELECT COUNT(*) AS DataCount
            FROM #TempPreMemberLeftJoin AS TPMLJ
            LEFT JOIN (SELECT DISTINCT TPB.PM_Sno , TPB.EP_CName , '黑名單' AS bk_list FROM CM_Pre_Member pm
            			INNER JOIN #TempPreBlacklist AS TPB
            			ON TPB.PM_Sno = pm.PM_Sno) AS TempTable
            ON TPMLJ.PM_Sno = TempTable.PM_Sno WHERE TPMLJ.PM_IsOk = 'Y' {whereSb}

            --條列出每個內容
            SELECT TPMLJ.* , bk_list , CASE WHEN TPMLJ.BossID IS NULL OR TPMLJ.BossID = '' THEN '(無主管資料)' ELSE TPMLJ.BossName END bkcname
            FROM #TempPreMemberLeftJoin AS TPMLJ
            LEFT JOIN (SELECT DISTINCT TPB.PM_Sno , TPB.EP_CName , '黑名單' AS bk_list FROM CM_Pre_Member pm
            			INNER JOIN #TempPreBlacklist AS TPB
            			ON TPB.PM_Sno = pm.PM_Sno) AS TempTable
            ON TPMLJ.PM_Sno = TempTable.PM_Sno WHERE TPMLJ.PM_IsOk = 'Y' {whereSb}

            DROP TABLE IF EXISTS #TempPreMemberJoin
            DROP TABLE IF EXISTS #TempPreMemberLeftJoin
            DROP TABLE IF EXISTS #TempPreMember
            DROP TABLE IF EXISTS #TempBlacklist
            DROP TABLE IF EXISTS #TempPreBlacklist
```

### 修改

```sql
	   --使用union all 取代每個or後 巢狀迴圈消失 執行時間縮短為4秒
           --再來能明顯增加速度優化方向為不使用暫存表
            SELECT TPMJ.* , TB.AutoNo , TB.CName , TB.PM_Tel1 AS T1 , TB.PM_Tel2 AS T2 , TB.PM_Tel3 AS T3
            FROM #TempPreMemberJoin TPMJ JOIN #TempBlacklist TB
            ON TPMJ.PM_Fax1 = TB.PM_Tel1
	    union all
	    SELECT TPMJ.* , TB.AutoNo , TB.CName , TB.PM_Tel1 AS T1 , TB.PM_Tel2 AS T2 , TB.PM_Tel3 AS T3
            FROM #TempPreMemberJoin TPMJ JOIN #TempBlacklist TB
            ON TPMJ.PM_Fax2 = TB.PM_Tel1
```
