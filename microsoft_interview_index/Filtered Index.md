Filtered Index（篩選索引）是**一種非聚簇索引（nonclustered index）**，但**只針對資料表中特定條件的資料列建立索引**，而非整張表。

---

## ✅ 優點

1. **改善查詢效能與執行計畫品質**
    
    - 篩選索引只包含特定資料列，**體積小、查詢快**。
        
    - 統計資訊（statistics）也更準確，因為只針對篩選條件的資料。
        
2. **降低索引維護成本**
    
    - 當 DML 操作（INSERT、UPDATE、DELETE）只影響部分資料，Filtered Index 只在需要時維護。
        
3. **節省儲存空間**
    
    - 取代整表索引，**對不同條件建立多個小型索引**，可以更有效使用儲存空間。


|資料特性|範例說明|
|---|---|
|有很多 NULL 的欄位（Sparse Column）|`WHERE EndDate IS NOT NULL`|
|異質類別資料（Heterogeneous Data）|Product 類型為 Accessories（`WHERE ProductSubcategoryID BETWEEN 27 AND 36`）|
|僅需查詢某些區段（範圍條件）|`WHERE Date BETWEEN '2024-01-01' AND '2024-01-31'`|


## 注意事項

- 若查詢的條件**不同於 Filter 條件**，必須將 filter 表達式中使用到的欄位**加入索引**（作為 key 或 included）。
    
- 若查詢要 SELECT Filter 條件的欄位，也必須將該欄位加入索引。
    
- 若 Filtered Index 涵蓋了表的大部分資料，維護成本可能高於完整索引。