# 共通處
- 兩者都使用相同的 SQL Database Engine、支援最新的字典相容模式（Azure 最低 100、Fabric 支援最高 160）
- 都提供如自動調整、自動備份、分割、Templating、Graph、JSON、XML 等核心功能


# 主要差異
[[PAAS & SAAS 交易型資料庫]]

| 比較項目        | Azure SQL Database                                    | SQL database in Microsoft Fabric                              |
| ----------- | ----------------------------------------------------- | ------------------------------------------------------------- |
| **定位**      | 通用 PaaS 交易型資料庫                                        | Fabric 中的 SaaS 交易型資料庫，與 OneLake 分析整合                          |
| **計算資源**    | 可選最大 128 vCores、雲端儲存支援高達 128 TB                       | Preview 階段最高 32 vCores、最多 4 TB 儲存                             |
| **可用性 SLA** | 99.995%（含區域冗餘）                                        | Fabric 支援 zone redundancy SLA，備份保留 7 天                        |
| 支援          | Always Encrypted、應用角色、C﻿DC                            | 無                                                             |
| 大規模匯入       | 支援從 Blob 的 BULK INSERT                                | Fabric SQL 尚未支援                                               |
| 備份策略        | Azure SQL 可選 LRS／ZRS／GRS<br>保留 1–35 天並支援長期備份（最長 10 年） | Fabric SQL 預設 ZRS<br>若區域不支援則降為 Local‑Redundant Storage (LRS)  |
| 複寫與副本       | Azure SQL 支援 0–4 個 HA 載具（只讀副本）與 0–4 Geo 複本            | Fabric SQL 提供讀取的 SQL analytics endpoint 用於分析，但不支援 geo-replica |
| 連線策略        | 可redirect 和proxy                                      | 僅支援 Redirect 模式                                               |
| 工具整合        | Azure CLI、PowerShell、BACPAC、SSIS、SSMS、sqlcmd、SSDT     | 可一鍵將資料鏡像到 OneLake，供 Spark、Notebooks、Power BI 分析使用             |





#microsoft_interview
