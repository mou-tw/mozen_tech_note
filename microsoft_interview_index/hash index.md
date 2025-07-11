- 適用於 In-Memory OLTP（記憶體內線上交易處理）資料表。
- 是一種 **記憶體優化的非聚簇索引（nonclustered hash index）**。
- 資料透過 **雜湊函數（Hash Function）** 對索引鍵進行映射，指向對應的 **Hash Bucket**。
- 每個 Bucket 是一個 **8 bytes 指標**，指向一串 **鏈結串列（linked list）**。

架構重點

| 元件                | 說明                                                                                          |
| ----------------- | ------------------------------------------------------------------------------------------- |
| **Hash Bucket**   | 每個 bucket 是一個 8-byte 指標，指向對應的 index key entries linked list。最大 bucket 數為 **1,073,741,824**。 |
| **Index Entry**   | 包含 index key 的值及對應資料列的記憶體位址（pointer）。多個 entry 以 linked list 串接在同一 bucket 下。                 |
| **Hash Function** | 系統內建且固定、可重複，會將相同的 key 對應到相同的 bucket。遵循 Poisson 分布（非均勻）。                                     |
BUCKET_COUNT 設定建議

|狀況|建議設定|
|---|---|
|最理想|`BUCKET_COUNT = 1~2 × distinct key 數量`|
|實務可接受|`BUCKET_COUNT ≤ 10 × 實際 distinct key 數`，**寧可多估不要低估**|
|預估困難|偏向使用 **Nonclustered Index** 取代|
|特別提醒|不會因增加 bucket 而減少 duplicated value 的鏈結長度|
Bucket 數量太少或太多的影響

|問題類型|效果|
|---|---|
|**太少 bucket**|- Hash Collision 增加- 鏈結串列變長，查詢變慢- Read 效能降低|
|**太多 bucket**|- 空 bucket 增多- 會浪費記憶體（每 bucket 8 bytes）- Index Scan 效能下降（掃空桶）|
 
效能最佳化原則

| 條件                     | Hash Index 效能表現           |
| ---------------------- | ------------------------- |
| `WHERE` 指定完整 key       | ✅ **表現最佳，支援 seek**        |
| `WHERE` 條件為範圍查詢（range） | ❌ 回退為 Index Scan          |
| `WHERE` 只指定部分 key      | ❌ 無法 hash → Index Scan    |
| key 重複多 or value 雜湊集中  | ⚠ 建議改用 Nonclustered Index |
| 小資料量（少於 bucket 數 1%）   | ✅ 表現良好，鏈結短                |