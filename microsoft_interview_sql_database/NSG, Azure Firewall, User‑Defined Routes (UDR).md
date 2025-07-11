
## Network Security Group (NSG)

層級：OSI 第 3–4 層（網路與傳輸層）

功能：依據源／目的 IP、埠、協定，允許或拒絕流量，採用 5 元組（5‑tuple）運作

部署場景：可套用在子網路或 VM NIC，非常輕量、設定簡單，適用於基本入站/出站控管

優點：免費、易管理、適合網段隔離

限制：不支援應用層過濾、FQDN tag、威脅偵測、DNAT/SNAT 等高階功



## Azure Firewall

層級：OSI 第 3–7 層（涵蓋網路、傳輸和應用層）
功能：提供狀態性檢測、深度封包檢查、威脅情報、防火牆規則、FQDN 過濾、URL 分類、DNAT/SNAT、TLS 檢查、IDPS 可用於 Premium SKU
部署場景：部署於虛擬網路邊界（Hub/Subnet），適合管理中央出入站或不同 VNet 間的高級控管
優點：高度安全、可偵測威脅與應用層控管；支援跨 VNet 統一管理
限制：成本高，需具備中高階設定與管理能力


## User‑Defined Route (UDR)

目的：自訂子網出口路由，覆寫 Azure 的預設系統路由或新增路由，控制出站流量的下一跳位置
**作用階段**：流量離開 VM 時，先通過 NSG 規則（決定放行與否），若放行，再檢查 UDR，決定路由至哪個下一跳（如 Firewall、NVA)

**用途**：配合 Firewall 或 NVA，強制流量經過安全檢查設備


## 比較表

| 項目        | NSG       | Azure Firewall    | UDR                  |
| --------- | --------- | ----------------- | -------------------- |
| OSI 層級    | 3–4       | 3–7               | 無檢查功能，只是路由           |
| 控制內容      | IP/埠      | IP/埠/FQDN/URL/威脅  | 路由下一跳目的地             |
| 狀態性       | 是         | 是                 | 無                    |
| 應用層過濾     | 無         | 支援                | 無                    |
| 威脅防護      | 無         | 有（Premium 可 IDPS） | 無                    |
| DNAT/SNAT | 否         | 有                 | 無                    |
| 適用場景      | VM、子網基本防護 | 中心防護＋日志＋威脅偵測      | 搭配 Firewall/NVA 路由控制 |
| 成本        | 免費        | 每小時 + 處理流量        | 免費，僅路由表              |
| 管理難度      | 簡單        | 複雜                | 中等（需設計）              |
## 部署方式
內部隔離和基本防護 → 使用 NSG

邊界安全和進階控管 → 配置 Azure Firewall

搭配 Firewall 執行路由控制 → 子網加上 UDR 將流量導入 Firewall 或 NVA


## 維安架構思路 (Defense‑in‑Depth)
第一道：NSG 控制進出子網流量

第二道：UDR 導向 Firewall 進行深層檢查與威脅防禦

Firewall：應用層檢查、威脅情報、防攻擊、日誌記錄





#microsoft_interview
