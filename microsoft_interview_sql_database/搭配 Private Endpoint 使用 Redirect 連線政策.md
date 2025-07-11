[[private link and endpoint]]
# 1. DNS 解析與流量導引
- 客戶端查詢例如 mydb.database.windows.net
- 由 Azure Private DNS Zone 負責解析該域名到 mydb.privatelink.database.windows.net
- 再解析至 Private Endpoint 的 私有 IP（如 10.0.x.x）
客戶端因此直接將流量導向相同 VNet 或跨 VNet 的 Private Endpoint。

# 2. TCP 連線建立與 Redirect 過程
- 客戶端對 Private Endpoint 的私有 IP 連線 TCP 1433
- 第一階段，連線建立與 Gateway 建立控制連線，Gateway 驗證後傳回資料庫叢集的私有虛擬 IP
- 隨後客戶端轉向該虛擬 IP、新的port（11000‑11999 之間）完成後續連線與資料傳輸
換言之，先經 Gateway 再被重導至資料庫節點，後續通道直接直通。

# 3. 必要網路閘道與埠設定
為使 Redirect 成功，需在 VNet 及 NSG 或防火牆中允許：
- private endpoint
  入站與出站均允許 TCP **1433 – 65535**
- **客戶端端點所在 VNet/VM**：
  出站允許 TCP **1433及11000–11999**（使用 Service Tag `Sql` 會更簡潔）
若埠未開放完整範圍，將退回使用 Proxy 模式

# 客戶端驅動支援
用以下 支援 Redirect 的 SQL 驅動程式：
ODBC、OLEDB
.NET SqlClient/Data Provider
Core .NET SqlClient Provider
JDBC v9.4 以上
**其他舊驅動**，即便設定 Redirect，仍會退回 Proxy 模式。


流程圖
```

[Client App in VNet]
   ⇓ DNS 查詢 mydb.database.windows.net
—解析→ privatelink 子域 → 私有 IP (PE)
   ⇓ TCP 1433 建立連線
[Private Endpoint NIC]
   ⇓ Gateway 確認後傳回資料庫叢集虛擬 IP
   ⇓ TCP 換埠至 11000–11999 建立直通連線
[DB Cluster Node] ←– 直接通道傳送與接收資料

```

Redirect 流程與流量走向整理
解析 → private IP
TCP 連入端口 1433 → Gateway 驗證
Gateway 回傳內部虛擬 IP + port
客戶端重新連線至該虛 IP + port → 與資料庫節點直通
資料通道不再走 Gateway




#microsoft_interview
