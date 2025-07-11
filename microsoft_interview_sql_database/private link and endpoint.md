Azure Private Link（私有連結）
Azure Private Link 允許你將服務（如 Azure SQL、Storage）連接到你的 VNet 中的 私有端點（Private Endpoint），使流量使用 Azure 骨幹網絡，不經公開網際網路
Private Endpoint 是你的虛擬網絡中的 NIC
## 使用 Private Link 的好處

**安全隱蔽**：資源不需要公開 IP，降低對外暴露風
**專屬私網流量**：流量全在 Azure 骨幹網絡內部傳輸，延遲低且穩定
**資料洩漏隔離**：Private Endpoint 僅映射特定資源，避免其他資源被存取
**跨網域與跨區域**：支援 VNet Peering、ExpressRoute、VPN 等方式，甚至跨區 Private Endpoint 存取
**支持自建服務**：你也能將自有應用透過 Private Link Service 提供給其他 VNet 私密使用

## DNS尋址
|方法|使用情境|設定難易|可擴充性|
|---|---|---|---|
|Private DNS Zone|Azure VNet 原生解析|⭐ 簡單快速|支援 VNet、VPN、ExpressRoute|
|自訂 DNS + Conditional Forwarder|混合雲、自建 DNS|⭐⭐ 中等複雜|可控制、但需維護|
|Hosts 檔 Overrides|測試用|⭐ 易設定|不適合正式環境|




#microsoft_interview
