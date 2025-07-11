常用 Metadata Views/DMVs

| 視圖 / DMV                                                                                                  | 主要用途                      |
| --------------------------------------------------------------------------------------------------------- | ------------------------- |
| `sys.indexes`, `sys.index_columns`                                                                        | 查看索引結構與包含欄位               |
| `sys.dm_db_index_usage_stats`                                                                             | 分析索引的使用頻率與效益              |
| `sys.dm_db_index_physical_stats`                                                                          | 偵測碎片與頁面資訊，以決定重建/整理        |
| `sys.dm_db_index_operational_stats`                                                                       | 觀察索引的維護及修改負載              |
| `sys.column_store_row_groups`, <br>`sys.column_store_segments`<br>`sys.column_store_dictionaries`         | 評估 columnstore 特定段落與數據範圍  |
| `sys.dm_db_column_store_row_group_physical_stats`<br>`sys.dm_db_column_store_row_group_operational_stats` | 進行 columnstore 儲存與修改的狀態監控 |
## 1. 索引結構與欄位資訊
`sys.indexes` + `sys.index_columns`
用途：查詢資料表中的所有索引、key 和 included 欄位。
