- 資料實體存放方式
  Clustered Index 以 B+ 樹結構儲存資料，葉節點即為實際資料頁，其上層則為索引頁
- 純一性與唯一性
  每張表最多只能有一個 clustered index（因為資料只能排序於一種方式）。若建立 primary key constraint，系統預設會在該欄建立 unique clustered index
- 唯一化處理
  若 clustered index 未設定 UNIQUE，SQL Server 會自動加入 4-byte hidden “uniqueifier” 欄，以保證 key 值唯一性

##   Query considerations
- Return a range of values by using operators such as `BETWEEN`, `>`, `>=`, `<`, and `<=`.
  使用聚集索引找到包含第一個值的行後，後續索引值的行將保證物理相鄰
- Return large result sets.
- Use JOIN clauses; typically these are foreign key columns.
- Use `ORDER BY` or `GROUP BY` clauses.
  may by pass 


### column considerations
適用
- 唯一或包含多個不同的值
  ID, (LastName、FirstName、MiddleName)
  PK自動
  _uniqueidentifier_ 不適合，if use> non cluster
- 有序訪問
  產品編號
- Ever‑increasing（持續遞增）
  使用 IDENTITY 或遞增型 key 能避免隨機插入造成碎裂，提升插入效能與降低 fragmentation 。
- 經常有sort需求

不適用
- 頻繁變動的欄位
- Wide keys（寬鍵）
  例如：多個大尺寸欄位組合、大 text 欄位等，會導致所有 nonclustered index 都被複製變大，影響效能
- **GUID/uniqueidentifier** 欄位（尤其使用 NEWID() 隨機值）
  雖然保證唯一性，但因 16 byte 大小且隨機性高，會造成 page split 與 fragmentation，降低效能。

