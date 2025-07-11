- **Columnstore Index（欄格式索引）** 是一種將資料以「欄為單位」儲存的技術，與傳統的 Rowstore（列儲存）不同。
    
- 特別適合用於 **分析型查詢**（OLAP）和 **大數據查詢**，可大幅減少 I/O 及壓縮資料大小。
    
- SQL Server 中主要有兩種：
    
    - **Clustered Columnstore Index（聚簇欄格式索引）**
        
    - **Nonclustered Columnstore Index（非聚簇欄格式索引）**



|元件|說明|
|---|---|
|**Rowstore**|傳統的 B+ Tree 或 Heap 結構，資料以「列」儲存。|
|**Columnstore**|資料以「欄」儲存，壓縮效率高，查詢特定欄位速度快。|
|**Delta Store（增量儲存區）**|當資料太少無法壓縮時，暫存在 rowstore 格式的 delta rowgroup 中（B+ Tree 形式）。|
|**Rowgroup**|columnstore index 的儲存單位，約 1,048,576 筆列資料一個 rowgroup。|
|**Column Segment**|每個 rowgroup 中，每個欄位都有一個 column segment。欄段是 columnstore 的壓縮單位。|

混合使用 Rowstore + Columnstore


| 組合方式                                              | 說明                                                                  |
| ------------------------------------------------- | ------------------------------------------------------------------- |
| ✅ Rowstore Table + Nonclustered Columnstore Index | OLTP 作業（更新/交易）可用 rowstore，OLAP 查詢可用 columnstore。                    |
| ✅ Columnstore Index + Nonclustered Rowstore Index | 允許在 columnstore 上建立 rowstore index（如 primary key 限制或 lookup index）。 |
