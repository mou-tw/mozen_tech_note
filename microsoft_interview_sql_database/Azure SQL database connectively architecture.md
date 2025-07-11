[Connectivity architecture - Azure SQL Database and SQL database in Fabric | Microsoft Learn](https://learn.microsoft.com/en-us/azure/azure-sql/database/connectivity-architecture?view=azuresql)

# 概述
描述direct traffic to Azure SQL Database的不同components
[[Redirect 和Proxy 模式]]
不論何種模式都要預留 TCP ports 1434 and 14000-14999 to enable Connecting with DAC.
[[DAC(Dedicated Administrator Connection)]]
## 適用情境
- Azure SQL Database
- SQL database in Microsoft Fabric
  [[Azure SQL Database & Fabric SQL]]
## 不適用情境
- Azure SQL Managed Instance
  [[Azure SQL Managed Instance]]
  [[Azure SQL Managed Instance Connectivity architecture]]
- decicated SQL pools in Azure Synapse Analytics.
  decicated SQL pools是屬於 Synapse Workspace 的一部份，管理著整體 Workspace 架構，Gateway 設計固定為proxy模式。
  為簡化客戶設定，Azure 保持在 Dedicated SQL Pools 上統一使用 Proxy，客戶不需考慮網路配置與節點動態 port
  無法變更 Private/Public Endpoint 的政策，Proxy 模式提供單一 TCP 1433 端口即可，避免了需要額外開放 11000~11999 埠的繁雜操作。
  適合 BI 報表導向工作負載：因為是以 OLAP 和批次計算為主，Proxy 模式的效能通常足以應付分析負載。
- connection strings to Azure SQL Database
  Azure SQL Database 的 connection string 本身 **不包含 `Redirect`** 或 `Proxy` 的設定選項，因為這是**伺服器層級**（Connection Policy）的行為，而非可透過客戶端文字串指定的內容
- control connectivity to the logical server for Azure SQL Database
  [[logical server]]
## Gateway migration

### 原因
**架構更新**：Micro­soft 周期性更新硬體與地區 Gateway，將流量導向較新、更強壯的節點。
**逐步淘汰舊 Gateway**：完成硬體升級後，逐步淘汰並關閉舊有 Gateway IP

### 會受到什麼影響？

DNS 解析結果變動：Azure SQL 的 FQDN 會解析到新的 Gateway IP。
硬編碼 IP 風險：若使用者在防火牆規則或 hosts 文件中硬編 IP，連線可能中斷。
如果使用Redirect & Service Tags則幾乎不受影響

## Azure SQL database, MI和SQL database in Microsoft Fabric的差別
### 架構定位與服務型態
- **Azure SQL Database**：純粹的單一資料庫 PaaS，適合雲端原生應用，支援單一資料庫與彈性池模式；不具備 Instance 層級設定。
- **Azure SQL Managed Instance**：介於 PaaS 與完整 SQL Server 之間，支援完整 Instance 層級功能（如 cross-DB、Agent、Linked Servers），部署於 VNet，適合 lift-and-shift。
- **SQL Database in Fabric**：Microsoft Fabric 生態系統中基於 SQL Database Engine 的 serverless 資料庫，集成 OneLake 輸出 Delta parquet，並採購 Fabric 容量套件計費。
### 功能性
| 功能特性                       | SQL Database | Managed Instance       | Fabric SQL Database                                                                                                                                                                                                    |
| -------------------------- | ------------ | ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| T-SQL 功能支援                 | 多數           | 幾乎完整，包括 cross-DB       | 多數，但 cadence may differ ([learn.microsoft.com](https://learn.microsoft.com/en-us/azure/azure-sql/database/features-comparison?view=azuresql&utm_source=chatgpt.com "Azure SQL Database & Azure SQL Managed Instance")) |
| Cross‑DB / Linked Servers  | ❌ 不支援        | ✅ 支援                   | ❌ 不支援                                                                                                                                                                                                                  |
| SQL Agent / Job Scheduling | ❌ 無          | ✅ 支援                   | ❌ 無                                                                                                                                                                                                                    |
| CLR、Service Broker、DTC 等   | ❌ 大多不支援      | ✅ 支援                   | ❌ 無                                                                                                                                                                                                                    |
| Elastic jobs、Auto-tuning   | ✅ 支援         | ✅ 支援                   | ✅ 支援 Fabric-native                                                                                                                                                                                                     |
| 數據同步/複製                    | Geo-replica  | MI Link、Replication 支援 | OneLake mirroring                                                                                                                                                                                                      |

### 部署與網路隔離
- **SQL Database**：支援公網、防火牆規則、VNet Service Endpoint、Private Link。
- **Managed Instance**：部署於隔離子網，支援 VNet-local、Public、Private Endpoint。
- **Fabric SQL DB**：屬於 Fabric Workspace，採 serverless 模型，支援 public access（受限）與 Private Link，但不支援 peering/MI-hosted VNet 模式
  
### 適用場景建議
- **雲端原生/新應用、高彈性+成本敏感** → 使用 **Azure SQL Database**
- **需要高 SQL Server 相容性、跨 DB、SQL Agent** → 選擇 **Managed Instance**
- **在 Fabric 生態中使用與 OneLake 串接、serverless 自動伸縮** → **Fabric SQL Database** 是理想選擇。

|需求項目|選擇建議|
|---|---|
|單一資料庫託管、高彈性與可用性|Azure SQL Database|
|舊系統 lift-and-shift、高相容性|Azure SQL Managed Instance|
|在 Fabric 中需與 Analytics 及 OneLake 無縫整合|Fabric SQL Database|
|資料倉、大規模 ETL 不適合此角色|選擇 Azure Synapse 其他方案|




#microsoft_interview
