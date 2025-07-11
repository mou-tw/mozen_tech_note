### 1. **瞭解資料庫的類型與特性**

#### OLTP 系統（線上交易處理系統）：

- 特徵：頻繁的資料寫入與修改、高吞吐量需求。
- 建議：使用 **Memory-Optimized Tables（記憶體優化表）** 與對應的索引（如非叢集記憶體索引、Hash 索引），可提供無鎖（latch-free）的效能優勢。

#### OLAP / DSS 系統（數據倉儲與決策支援系統）：
- 特徵：處理大量資料，重視查詢效率。
- 建議：使用 **Columnstore Index（直欄式索引）**，適合執行篩選、彙總、群組、星型查詢（star-join）等分析型任務。



### 2. **分析最常被執行的查詢語句**    
- 例如：如果常見查詢是多表 JOIN，就需要根據 JOIN 條件來設計相關索引，以提升查詢效率。
  ex:
```

SELECT o.OrderID, o.OrderDate, c.CustomerName
FROM Orders o
JOIN Customers c ON o.CustomerID = c.CustomerID
WHERE o.OrderDate >= '2024-01-01'

```

- **Customers.CustomerID**：在 `JOIN` 條件中，是主鍵，預設已有聚集索引（不需額外建立）。
- **Orders.CustomerID**：需建立非叢集索引，用於 JOIN。
- **Orders.OrderDate**：若經常用於篩選，可以納入複合索引或建立單獨索引。

```

CREATE NONCLUSTERED INDEX IX_Orders_CustomerID
ON Orders (CustomerID);

CREATE NONCLUSTERED INDEX IX_Orders_OrderDate
ON Orders (OrderDate);

or
或使用複合索引（若 `WHERE` 和 `JOIN` 同時常用）：

CREATE NONCLUSTERED INDEX IX_Orders_CustomerID_OrderDate
ON Orders (CustomerID, OrderDate);

```


### 瞭解查詢中欄位的特性
針對以下類型的欄位建立索引：
- 整數型別
- 唯一值（UNIQUE） 或 非 NULL
若欄位中只包含特定值範圍（例如某欄只有「有效」與「無效」），可以使用：
**Filtered Index（篩選式索引）**：只針對符合條件的資料建立索引，有效減少儲存空間與維護成本。