Azure SQL Managed Instance會被放置於azure VNET和subnet中

## 透過vnet-local endpoint訪問virtual cluster
1. DNS 解析與初始 TCP 連線
   應用程式解析 FQDN，取得 **子網內部 Load Balancer 的私有 IP**
   應用程式向此 IP 發起 TCP 連線（TCP 1433）。
2. Load Balancer 與 Gateway 角色
   Load Balancer 接收連線，將請求導向 Virtual Cluster 中負責此 MI 的 **internal gateway**
   Gateway 接收初次連線，進行身份驗證與策略判斷（Redirect 或 Proxy），透過 FQDN 中的名稱定位具體資料庫的節點。
3. Redirect 或 Proxy 資料通道建立
   Redirect 模式（建議）
   Gateway 傳回 實際節點的虛擬 IP + 動態 port（11000–11999），應用程式轉向新 IP 再次建立連線，之後直連該節點，跳過 gateway 中繼
   Proxy 模式
   所有資料流持續經 gateway 中轉，流量維持於初始建立的 session 中 。


## VNet Peering、VPN/ExpressRoute、Private Endpoint 使用場合


|方式|使用情境|模式|優勢|
|---|---|---|---|
|**VNet Peering**|多個 Azure VNet 間通訊|Redirect 或 Proxy|延遲低、成本低、支援多種服務|
|**VPN/ExpressRoute**|本地➡Azure / VNet 跨網連線|Redirect 或 Proxy|安全穩定、適用企業級互通|
|**Private Endpoint**|單 VNet 對 MI 單點私密存取|Proxy|IP 穩定、隔離性高、配置簡易

### VNet Peering（虛擬網路對等連線）
- 相同或跨區 VNet 間互通：兩個 VNet（包含 MI 和 App 的 VNet）彼此建立 Peering，可以是跨訂閱或跨區域。
- 流量走 Azure 背骨網路：無需 NAT 或 VPN，加密內部流量延遲低、效能高
- 優點：簡單彈性、成本低、適用於多種 Azure 資源通訊。
### VPN Gateway or ExpressRoute（混合網路）
 - 與本地或其他 Azure VNet 連線：
   可透過 Site‑to‑Site VPN 或 ExpressRoute 將內部網路／其他 VNet 與 MI 所在 VNet 連通
 - 流量封閉於專線/VPN:
   相較於公網，可提供更高安全性與穩定性
 - 適用於本地資料中心整合、大規模組織架構
 - 若想讓 App Service 使用該連線須另配置 App VNet Integration 或跳板機
### Private Endpoint
- **針對應用 VNet 建立一個私有 IP**：
  一旦建立，Azure SQL MI 在對應 App VNet 中有一個固定私有 IP 地址（port 1433）
- 單向鏈接，無需 Peering 或 VPN：
  只允許這個 VNet 的應用程式對 MI 發起連線，不會反向暴露 MI 至 VNet 。
- 只占一個 IP
  在你指定的子網（subnet）內，Azure 會為該 MI 建立一個 Network Interface，並分配一個私有 IP 位址。這個介面即是應用程式連至 MI 的唯一入口點。無論後端 MI 的節點如何分佈，Private Endpoint 都扮演單一中介節點（proxy gateway）。所有流量都經過這個 IP 對應的代理，再轉發至 MI 內部真正的引擎節點 。
- 優勢：
  IP 穩定、網路隔離性強、有效避免 IP 重疊問題 
- 僅支援 Proxy 模式（不支援 Redirect）

###  配置建議
- 若 App 在同一區域 VNet 或 Peering VNet → 優先使用 **VNet Peering + Redirect**。
- 若 App 在公司內部 → 選擇 **VPN 或 ExpressRoute + VNet-local endpoint + Redirect**。
- 若 App 僅需存取 MI，且無全 VNet 互通需求 → 用 **Private Endpoint（Proxy）** 即可。

