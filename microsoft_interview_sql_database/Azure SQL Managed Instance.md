Azure SQL Managed Instance 是一種全託管、內建 VNet 的 PaaS 服務，具備與 SQL Server 本機版本極高的相容性，且提供企業級的隔離、安全與擴展能力，適合追求全面 SQL Server 功能、企業隔離能力與混合整合的用例。它結合了傳統資料庫的完整性與雲端 PaaS 的便利性。


部署於 隔離 VNet 子網，支援 Private Endpoint 和 VNet-local 存取方式，確保環境安全


## data plane and control plane
|平面類型|功能重點|使用權限|
|---|---|---|
|**Control Plane**|部署、升級、縮放、遷移、備份等管理操作|由 Azure Backend 控制 agent 執行，使用者無直接存取權|
|**Data Plane**|資料查詢、更新與用戶端實際請求|部署於 VNet 子網內，由使用者應用程式透過端點存取|
### 控制平面（Control Plane）

控制平面承擔部署、配置、維護、升級、調整規模、備份、管理等架構和生命周期操作
通過 Azure Resource Manager（ARM）呼叫進行作業。這些請求由 MS 控制 agent 於專屬虛擬機集群上執行，並使用 TLS 加密+簽章以確保安全性。你無法直接存取（如 SSH/RDP）這些節點
### 資料平面（Data Plane）
處理實際的資料讀寫請求，如 T-SQL 查詢、SP 執行等。這平面會直接與資料庫引擎互動，傳輸使用者的資料。
完全部署在你的 VNet 子網內，透過 Load Balancer + Gateway 串連至資料節點，並支援 VNet-local 入口端點



## 通訊端點

在 Azure SQL Managed Instance (MI) 中，您可以透過三種不同的端點類型來連接資料庫，分別為：VNet-local endpoint、Public endpoint 以及 Private endpoint。\

|端點類型|存取範圍|通訊埠|連線型態|主要用途|
|---|---|---|---|---|
|VNet-local Endpoint|同一 VNet 內|1433|Proxy/Redirect|高效能內部連線，支援複雜功能（如 Failover Groups）|
|Public Endpoint|來自互聯網|3342|Proxy|外部應用程式連線，無需 VPN 或 Peering|
|Private Endpoint|指定 VNet 內|1143|Proxy|高安全性需求，避免公開 IP 連線|

### VNet-local Endpoint（預設端點）
- **格式**：`<mi_name>.<dns_zone>.database.windows.net`
- **存取方式**：僅限於與 MI 部署於同一虛擬網路（VNet）中的資源。
- **通訊埠**：預設使用 TCP 埠 1433。
- **連線型態**：支援 Proxy 與 Redirect 兩種模式。
    - **Proxy**：所有連線流量經由內部閘道器處理，適用於舊版 TDS 驅動程式。
    - **Redirect**：連線建立後，流量直接導向資料節點，提供較低延遲與更高效能，建議使用此模式。
- **適用場景**：適用於部署於同一 VNet 中的應用程式，或需要高效能連線的情境。
- **注意事項**：若需跨 VNet 存取，需透過 Private Endpoint 或 VNet Peering 等方式實現。

### Public Endpoint（公開端點）
- **格式**：`<mi_name>.public.<dns_zone>.database.windows.net`
- **存取方式**：允許來自互聯網的連線。
- **通訊埠**：使用 TCP 埠 3342。
- **連線型態**：僅支援 Proxy 模式。
- **適用場景**：適用於需要從外部（如 Azure App Service、Power BI、或其他雲端服務）連接 MI 的情境。
- **注意事項**：建議僅在無法透過 VNet-local 或 Private Endpoint 連線時使用，並應搭配適當的網路安全性群組（NSG）規則以限制存取

### Private Endpoint（私有端點）
- **格式**：`<mi_name>.privatelink.<dns_zone>.database.windows.net`
- **存取方式**：透過 Azure Private Link 技術，將 MI 以私有 IP 的形式暴露於指定的 VNet 中。
- **通訊埠**：使用 TCP 埠 1143。
- **連線型態**：僅支援 Proxy 模式。
- **適用場景**：適用於需要高安全性與隔離性的情境，如混合雲架構、跨租戶存取、或需避免公開 IP 的情境。
- **注意事項**：每個 Private Endpoint 僅支援單向連線，且僅限於 TCP 埠 1433 的流量。

## Virtual cluster connectivity architecture

當你建立一個 SQL Managed Instance（MI）時，Azure 會在你指定的 VNet 子網中，自動建立一個 **Virtual Cluster**，這是一組由多台專屬 VM 主機構成、用於托管 SQL Instance 的叢集（cluster）集群
- **Single-subnet 限制**：Virtual Cluster 必須放在專用於 MI 的子網內，不與其他資源共用 。
- **一叢集多實例**：每個子網僅能包含一個 Virtual Cluster，內可部署多個 Managed Instance 。
### 連線流程分解

1. DNS 解析與 Load Balancer（LB）
   應用程式解析 MI FQDN（如 `<mi>.dnszone.database.windows.net`）得到子網中虛擬負載平衡器（ILB）的私有 IP
2. 第一階段流量：ILB → Gateway
   ILB 將 TCP 連線（通常由 port 1433 進入）導向叢集中用於 client 入口的 **gateway 節點**。此 gateway 處理初次驗證、授權與 session 建立\
3. 選擇連線策略：Proxy 或 Redirect

## Network requirements
### 子網配置要求（Subnet Requirements）

MI 必須部署在 **專用子網 (dedicated subnet)** 中，且符合以下條件：

- 僅能包含 MI 與其所需資源，不可與其他服務（如 VM、APIM）共享同一子網，也不能是 Gateway Subnet
- 必須對該子網執行Microsoft.Sql/managedInstances委派（Subnet Delegation）
- 至少有 **32 個可用 IP 位址**；推薦最少 32
- 必須關聯 Network Security Group（NSG）與 Route Table（UDR），並允許 MI 服務自行新增必要規則

### NSG rules

|方向|埠號範圍|用途|
|---|---|---|
|**Inbound**|TCP 1433|客戶端連線資料平面|
|**Inbound**|TCP 11000–11999|Redirect 模式下，節點資料流量（於 VNet-local）([learn.microsoft.com](https://learn.microsoft.com/en-us/azure/azure-sql/managed-instance/connection-types-overview?view=azuresql&utm_source=chatgpt.com "Azure SQL Managed Instance connection types - Learn Microsoft"))|
|**Inbound**|TCP 5022|若使用 MI Link 同步本地 SQL（linked-server）|
|**Outbound**|同樣為 1433 & 11000–11999|對其他 Azure SQL VM 或資料庫節點、MI 內部引擎節點進行外部呼叫|
|**Outbound**|TCP 5022|MI 同步本地資料時|
|**Outbound**|TCP 25 或 587|若使用 Database Mail 功能（根據租戶支援）|

Azure 會自動注入處理 MI 管理與控制平面所需的 NSG/UDR 規則，不建議手動覆蓋這些條目

### Route Table（UDR）需求
- 必須關聯一張 Route Table 到子網
- 若你需要透過 Azure Gateway (VPN、ExpressRoute) 或 NVA（虛擬網路設備）進行流量轉導，可自定 UDR
- 特別注意：Azure SQL 會自動新增對其管理流量所需的路由條目，若自定 UDR 與其衝突，可能導致服務異常
### 混合環境 VLAN & ExpressRoute/VPN 需求

若 MI 與本地 SQL 伺服器進行連線（例如 MI Link 或 transaction replication）：
- 必須開放 TCP 5022（MI Link 對等），如 HTTPS over TCP
- 同時允許 11000–11999 範圍以支持 Redirect 模式
若使用 **VPN Gateway 或 ExpressRoute**：
- 保證 VNet 至本地或另一 VNet 的連通
- 避免 DNS override 或 UDR 導致 Gateway route 被覆蓋
- **App Service** 若要存取 MI，需使用 VNet Integration 配合「允許 gateway 傳遞」或 Private Endpoint

