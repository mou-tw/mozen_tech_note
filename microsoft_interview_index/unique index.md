## Unique Index 設計指南整理

### 1. 什麼是 Unique Index

- 保證索引鍵（key）中的值**不會有重複**。
    
- 因此，資料表中的每一列都在某種程度上是唯一的。
    
- 適用於資料本身具唯一性的欄位。
    

### 2. 範例說明

- 如 HumanResources.Employee 表的 `NationalIDNumber` 欄位，希望此欄位值唯一，但主鍵是 `EmployeeID`。
    
- 可在 `NationalIDNumber` 建立 UNIQUE 約束（constraint）。
    
- 若插入重複值，系統會阻止並回報錯誤。
    
- 多欄位 unique index：  
    例如建立在 `(LastName, FirstName, MiddleName)` 上，確保組合值唯一。
    

### 3. 聚簇與非聚簇 Unique Index

- Unique index 可為聚簇（clustered）或非聚簇（nonclustered）。
    
- 同一張表可同時有一個 unique clustered index 和多個 unique nonclustered indexes。
    

### 4. Unique Index 的好處

- **確保資料完整性**（Data Integrity）。
    
- 提供額外資訊給查詢優化器（Query Optimizer），幫助產生更有效的執行計畫。
    
- 建立 PRIMARY KEY 或 UNIQUE constraint 時，會自動建立 unique index。
    
- UNIQUE constraint 和手動建立 unique index 在資料驗證及優化器行為上無顯著差異。
    
- 若目標是維護資料完整性，應使用 UNIQUE 或 PRIMARY KEY constraint，使意圖明確。
    

### 5. 注意事項

- 若資料中已存在重複鍵值，無法建立 unique index 或 UNIQUE constraint。
    
- 若資料確實唯一且需強制唯一性，**建議建立 unique index（最好透過 UNIQUE constraint）**，優於非唯一索引。
    
- Unique nonclustered index 可以包含非鍵欄位的包含欄位（included columns）。