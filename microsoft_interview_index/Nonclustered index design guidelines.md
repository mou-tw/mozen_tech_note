 - 類似書籍索引，它儲存 key 值與指向資料列的定位欄（row locator），可快速定位資料，但資料本身儲存在聚簇索引或 heap。
 - 結構為 B+ 樹：葉節（leaf）儲存 key 與 locator，非leaf節（root/intermediate）幫助查找導引。
- 支援所有資料型別，**但不包含** `text`、`ntext`、`image`。
- **計算欄位（computed columns）** 只要是**確定性 (deterministic)** 且類型允許，也能放在 Included columns 裡。
- 欄位名稱**不能同時出現在 key 欄位與 INCLUDE 欄位**中，也不能在 INCLUDE 中重複。
## Nonclustered index architecture
非聚簇索引（Nonclustered Index）與聚簇索引都是以 B+ 樹結構（B+ tree）儲存在磁碟上，但它們在一起重要差異如下：
### 結構與葉節點差異
**Leaf Level（葉節點）內容不同：**
- **聚簇索引** 的葉節點為實際的資料頁：裡面存放整行資料。
- **非聚簇索引** 的葉節點則為「索引頁」：只存 key 欄位（和被 include 的非 key 欄位），不存完整的資料行。
**資料排序與存放順序：**
- 聚簇索引將整張表「依 key 排序」存放。
- 非聚簇索引不改變底層資料排列，它只是維護一棵獨立的索引樹，裡面依 key 排序，但實際資料散布（heap 或聚簇樹）保持原樣。
### Row Locator（列定位方式）
- **對 heap 表**（無聚簇索引）：非聚簇葉節點會儲存一個物理資料定位指標（RID），包含檔案 ID、頁號與行號。
- **對有聚簇索引的表**：葉節點會儲存聚簇索引 key 作為 row locator，之後再以該 key 去聚簇樹上查找完整資料。



## 設計建議
1. **針對常用查詢建立**
   應根據經常使用的 `WHERE`、`JOIN`、`GROUP BY`、`ORDER BY` 條件來設計索引 key。
2. **Key 欄位選擇：齊性 + 高選擇性**
   半唯一或高 distinct 值適合成為 key。
   避免選低 distinct 值（如性別）欄位，若需要可考慮 Filtered Index。
3. Include 欄位用於覆蓋 (Covering)
   使用 `INCLUDE(...)` 加入非 key 欄位至 leaf level，以避免需 lookup 聚簇索引。
   避免讓 key 欄位過寬影響 B+樹深度與 pointer 效能。
4. 保持索引小而精簡
   索引多、key 寬大會影響寫入效能（INSERT/UPDATE/DELETE）。
   在高寫入量表中，保持 key 少且窄；在主要為查詢表中，可多 key、多 include 但仍要控制大小。 
5. Avoid 限制與成本效益權衡
   小表（<1000 pages）偶爾 full scan 比用索引更快，因此可能不合適建立索引。
   索引上限：每表最高 999 個非叢集索引。用前要審慎衡量是否有真實需求。

## Allocation Units 三大類型
每個非叢集索引至少會為 **每個 partition** 建立一個 `IN_ROW_DATA` allocation unit（儲存 B+ tree 的頁面結構）。此外，根據欄位型態，有可能再加入以下兩種：

|Allocation Unit 類型|作用與用途|
|---|---|
|**IN_ROW_DATA**|儲存索引本身的索引頁（B+ tree），至少一個|
|**LOB_DATA**|若 index 包含 LOB 欄（如 `varchar(max)`、`xml` 等），會建立此 unit|
|**ROW_OVERFLOW_DATA**|當 variable-length 欄位（如 `varchar(n)`）單列值或整行資料超限 8060 bytes 時，超出部分會移到此 unit|

- **IN_ROW_DATA**：所有 B-tree 的結構與 leaf/page 皆儲存在這裡。
- **LOB_DATA**：支援 off-row LOB 個別儲存，索引 leaf 儲存 pointer 至這些資料而非整列。
- **ROW_OVERFLOW_DATA**：若 variable-length 欄位爆表，offload 溢出至此 unit，保留空間限制。

## 根據資料庫特性選擇索引策略

|項目|建議策略|
|---|---|
|Filtered Index|對特定子集頻繁查詢、null值多、資料不均的欄位使用。|
|Covering Index|加入 INCLUDE 的方式避免 Key lookup，可合併多個查詢需求到同一索引中。|
|Selectivity|選 key 欄位時應避免低 distinct，可依查詢加索引。|
|規模與維護|大表可多索引；小表或大量 DML 操作的表，應少索引、窄索引。|
|維護成本 vs 收益|新索引應衡量是否可合併現有索引以降低維護負擔。|


## 背景限制：非聚簇索引的 Key 限制
|限制項目|數值|
|---|---|
|最多 key 欄位數量|16|
|key 總大小（byte）|900|
這些限制只套用在**key 部分**，**`INCLUDE` 欄位不會計入**這些限制。


解決方案：使用 `INCLUDE` 包含非 key 欄位
將佔用空間大的欄位放入 `INCLUDE` 區段，可：
- 避免超過 key 長度或大小上限。
- 繼續支援「覆蓋查詢」（covering query），提升效能。

## 性能考量：避免加入過多不必要的欄位（key 或 nonkey）

### 1. **索引頁面容量降低**

- 索引欄位越多，每個索引頁面能存放的索引列數越少。
    
- 造成更多頁面需要被讀取 → **I/O 負擔增加**。
    
- 同時快取效率下降（cache hit ratio 變低），因為更多頁面被佔用。
    
### 2. **磁碟空間需求增加**

- 特別是包含大量字串或大型物件型態 (`varchar(max)`, `nvarchar(max)`, `varbinary(max)`, `xml`) 的非 key 欄位。
    
- 這些欄位的資料會**複製存放在索引的 leaf level**，同時也存在於基底資料表中 → 空間使用翻倍。
    
### 3. **索引維護成本提高**

- 資料修改（INSERT/UPDATE/DELETE）時，所有索引欄位都需要同步維護。
    
- 索引欄位越多，維護時間越長 → 寫入效能受影響。