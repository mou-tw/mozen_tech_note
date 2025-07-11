



### FILLFACTOR 设置
通过保留部分页空白来减少分裂：
```
CREATE INDEX IX_Orders_CustID
  ON Orders (CustomerID)
  WITH (FILLFACTOR=80);
```

会保留 20% 空间用于未来插入，降低叶页页分裂和碎片

### 存储位置与分区（Filegroups / Partition Schemes）

Filegroups（檔案群組）
是一組資料檔 (.mdf/.ndf) 的邏輯集合，可讓你將資料實體置於不同物理磁碟或 LUN 上

Partition Scheme（分割方案）
是配合 Partition Function 的物件，用以決定資料如何依欄位值（如 OrderDate）水平分割成多 partition，並將這些 partition 映射至不同檔案群組.
```
CREATE NONCLUSTERED INDEX IX_Orders_OrderDate
  ON Orders (OrderDate)
  ON FG_Indexes; -- 存放于不同文件组
```
可将索引与表放置于不同磁盘，提升 I/O 并行度
当索引与表分别存储在不同的物理磁盘或阵列上时，SQL Server 可以同时从两个硬盘读取数据：
- **一个磁盘**处理索引扫描/查找
- **另一个磁盘**处理表数据访问（如 bookmark lookup 或聚集索引扫描）  
    这避免了单一磁盘的读写“来回争用”，实现真正的并行 I/O
若無法預測查詢模式，將資料與索引散布在所有 filegroup 上，可確保每個 filegroup 都有可能被使用，進而平均分攤 I/O 負載，且管理相對簡化 。
當索引跨多 filegroup 時，可針對不同部分執行重建或備份，縮短維護時間、支援分區修復 。

注意事项
若两个 filegroup 放在同一物理磁盘或阵列上，则不会带来性能提升
仅在硬件存在多个独立磁盘、控制器或 LUN 时有效。
若数据库或查询已是 CPU/内存瓶颈，而非 I/O，则不会有明显改善。

## 跨多個檔案群組（filegroup）分割索引
**Partition function**  
定義如何根據分割欄位（如日期）將資料映射到具體分割。  
例如：
```

CREATE PARTITION FUNCTION PF_OrderDate(datetime)
  AS RANGE RIGHT FOR VALUES 
    ('2022-01-01','2023-01-01','2024-01-01');

```

Partition scheme
```

CREATE PARTITION SCHEME PS_OrderDate
  AS PARTITION PF_OrderDate 
  TO ([FG2022],[FG2023],[FG2024],[FGCurrent]);

```

應用至索引
建立聚簇或非聚簇索引時，指定使用 partition scheme：
```
CREATE TABLE Orders (
  OrderID INT PRIMARY KEY,
  OrderDate DATETIME,
  Amount DECIMAL(10,2)
) ON PS_OrderDate(OrderDate);

```

或為非聚簇索引指定 scheme：
```
CREATE NONCLUSTERED INDEX IX_Orders_Customer
  ON Orders(CustomerID)
  ON PS_OrderDate(OrderDate);

```