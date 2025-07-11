可分為三個層面
資料庫層級、查詢和column level

## 資料庫考量 (Database Level)
- 索引数量不要过多：大量索引会拖慢 INSERT、UPDATE、DELETE 等写入操作，因数据改动时必须同步维护所有相关索引
- 高更新频率表（如OLTP）建议少而窄的索引，避免写入开销
- 读多写少表（如OLAP）可多建立索引提升查询性能 。
- 小表不宜过度索引，因为全表扫描可能比索引查找更快 。
- 索引在视图上也有效，可为聚合、JOIN等复杂查询提供显著效能提升。
  在普通视图中，每次查询都会动态执行聚合或 JOIN 操作；而 indexed view 会 预计算 并将结果存储在磁盘上，类似于一张带聚集索引的物化表
  若视图定义中包含 SUM()、COUNT_BIG() 等聚合函数，系统会在索引中保留这些聚合的最新值。在运行类似查询时，可直接读取索引数据而无需再次执行聚合运算，从而大幅减少 CPU 和 I/O 开销
  缺點
  **维护成本提高**：对基表进行 DML 操作时，需要同时更新视图索引，可能影响写入性能
  **更新锁争用**：特别当多个事务操作影响相同聚合分组时，可能引发锁争用 。情境容易發生在：**高併發更新底層聚合維度資料表**，且多個交易共用同群組鍵，來源更新頻繁不建議
- **使用Azure等平台可启用自动索引调整及Query Store追踪历史执行计划和建议**。
  Query Store追踪历史执行计划和建议**
  Azure 门户 → Query Performance Insight
  提供顶部资源消耗查询列表、计划趋势对比、建议等
  自動索引調整
  数据库或服务器资源中选择 “Automatic tuning”，开启 `CREATE INDEX` 与 `DROP INDEX`
```
ALTER DATABASE CURRENT 
SET AUTOMATIC_TUNING = (
  CREATE_INDEX = ON,
  DROP_INDEX   = ON,
  FORCE_LAST_GOOD_PLAN = ON
);
```

## 查詢考量 (Query Level)
- 精确匹配条件适合索引，如 =、>、BETWEEN，优化器可高效利用索引。
- non cluster index放在常用的WHERE或JOIN列上（SARGable 列）
- covering index: 若索引同时包含 SELECT、WHERE、JOIN所需的所有列，查询只访问索引页，减少I/O，性能显著提升
  [[covering index example]]
- 單一SQL語句中，盡可能插入或者修改多的raw
  減少語句執行次數：一次性完成多筆操作
  交易成本降低：僅產生一筆交易日誌，提高效率
  索引維護優化：單次更新可減少索引頁面重建次數
  I/O 與鎖競爭降低：各種資源爭用減少，整體性能提升
```
insert 範例
INSERT INTO Employees (EmployeeID, Name, Salary, DepartmentID)
VALUES
  (1, 'Alice',   80000, 10),
  (2, 'Bob',     85000, 20),
  (3, 'Charlie', 90000, 10),
  (4, 'Dana',    95000, 30);

```

若要一次更新多筆資料，建議先將要更新的識別鍵與新值暫存至臨時表，再使用 JOIN 一次性更新：
```

-- 步驟 1：建立包含更新值的暫存表
CREATE TABLE #updateTemp (
  EmployeeID   INT PRIMARY KEY,
  NewSalary    INT
);

INSERT INTO #updateTemp (EmployeeID, NewSalary) VALUES
  (1,  82000),
  (3,  92000),
  (4, 100000);

-- 步驟 2：一次性更新多行資料
UPDATE e
SET e.Salary = t.NewSalary
FROM Employees AS e
JOIN #updateTemp AS t
  ON e.EmployeeID = t.EmployeeID;
```


### 欄位考量(Column Level)
- 索引鍵盡可能短，且最好是unique以及notnull
  聚集索引键使用 INT（4 字节）vs. 使用 UNIQUEIDENTIFIER（16 字节）  
  后者导致 B+ 树深度增加，更慢的查找与更大的页面占用
  若聚集键不是唯一的，SQL Server 会自动添加 4 字节的 **uniquifier** 来区分重複键值，进一步“加宽”键值，影响性能
- **避免将 LOB 类型（如 text, image, varchar(max)）作为索引键**，可以作为 non-key 列 include 在非聚集索引中使用
- xml data type can only be a key column only in an XML index.
- unique index的整数列或唯一列建立索引可以显著提高性能
```
CREATE UNIQUE NONCLUSTERED INDEX IX_Table_Email
ON Table (Email);
```
- **低基数列**（值稀少且重复多）则不建议单独建索引，很可能被查询优化器忽略并进行全表扫描。
  如性別
  如有傾斜狀況，或者只關注低基數列中的某值(訂單狀態中的failed)，選擇filter index
- Filtered Index（筛选索引）**：对特定条件子集建立索引，体积小，维护负载低
- 如果索引包含多個列，搜尋的columns應該放在最前面
  舉例
```

CREATE NONCLUSTERED INDEX IX_Contact_Last_First
  ON Person.Contact(LastName, FirstName);

```
	查询示例 & 索引使用：
	1.`WHERE LastName = 'Smith'` → **Index Seek**，效率高
	2.`WHERE LastName = 'Smith' AND FirstName LIKE 'J%'` → **Index Seek with range**, 也高效
	3. `WHERE FirstName = 'Jane'` → 只靠第二列条件，**Index Scan**，I/O 大幅增加



