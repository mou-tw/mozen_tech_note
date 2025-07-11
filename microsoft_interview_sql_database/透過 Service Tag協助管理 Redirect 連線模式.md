Service Tag 是 Azure 用來代表一組可變動 IP 前綴的代號（例如 `Sql`）。這些 IP 前綴由 Microsoft 自動更新，讓您不必手動維護一長串 IP 清單
https://learn.microsoft.com/en-us/azure/virtual-network/service-tags-overview


![[Pasted image 20250624165611.png]]

流程
- 透過在 NSG、防火牆、Azure Firewall 等規則中使用以下方式：
  [[NSG, Azure Firewall, User‑Defined Routes (UDR)]]
將流量目的標記為 Sql（或地區限定，如 Sql.Eastus）
- 選擇對應的埠：
  11000–11999（Redirect 模式）
  1433（Gateway 連線）
  
好處
- 一次性允許整組服務 IP → 不需管理每個 IP
- 自動獲得最新 IP 更新 → 減少維護工作





#microsoft_interview
