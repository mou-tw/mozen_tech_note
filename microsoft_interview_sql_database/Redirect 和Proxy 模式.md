在沒有使用 Direct Network Traffic（即沒有 Redirect 模式）時，Azure SQL Database 和 Microsoft Fabric 會以 Proxy 模式處理網路流量。

# Proxy 的網路運作方式
- 連線方式透過gateway
  客戶端透過公有 IP，連接到 Azure SQL gateway（監聽 TCP 1433），所有後續查詢及資料傳輸都 經由 gateway 中轉，直到資料庫伺服器。
- 特性
  登入、查詢、回傳結果、提交交易等，一律會經過 gateway。
  gateway 對所有通訊負責 TCP 轉送與安全控制
- 優點
  只需允許出站流量至 gateway 的 TCP 1433 埠即可連線
- 缺點
  延遲較高（每次請求都多一中轉）
  吞吐量受限於 gateway 容量
  若 gateway 發生維護或重新部署，會中斷現有連線 。

# Redirect的網路運作方式
- 僅在初次連線時透過gateway
  客戶端初次連接 gateway（TCP 1433），經過驗證後，gateway 會回傳目標節點的虛擬 IP ＋ 動態 port（11000–11999），後續請求會直接與節點通訊，不再經過 gateway 。
- 特性
  除 1433 外，還需允許 11000–11999 的出站連線（可使用 SQL Service Tag 來簡化）
  客戶端需支援 TDS 7.4 或更新（ADO.NET 4.5+、JDBC 4.2+、ODBC 11+）
- 優點
  延遲低（減少中轉點）
  吞吐量高（直連節點）
  gateway 維護不會中斷資料通訊 。
- 網路設定
    [[透過 Service Tag協助管理 Redirect 連線模式]]
    [[搭配 Private Endpoint 使用 Redirect 連線政策]]

# 預設設定
- 客戶端來自 Azure 內部 → 使用 Redirect
- 來自外部網路（internet）→ 使用 Proxy (可調整)

# 比較表

| 特性      | **Proxy**                | **Redirect**                      |
| ------- | ------------------------ | --------------------------------- |
| 資料流程    | client → gateway → DB 節點 | 初次經 gateway → 之後 client → DB 節點直連 |
| 開啟 Port | **1433**                 | **1433 + 11000–11999**            |
| 延遲      | 相對高，頻繁請求中轉一次             | 相對低，直接通訊                          |
| 吞吐量     | 受限，gateway 容量限制          | 高，無轉送瓶頸                           |
| 穩定性     | gateway 維護可能導致連線中斷       | 已連後不依賴 gateway，較穩定                |
| 客戶端需求   | 無特殊需求                    | 需支援 TDS 7.4+                      |
| 適用場景    | 不支援 redirect 的遺留應用       | Azure VM、私有端點、效果需求高的場景            |

# Proxy使用場景

客戶端不支援 TDS 7.4 或以上，無法接受 Redirect 指令
Deployment 設定了 Connection Policy = Prox
使用公共/private endpoint 的情況下，有時也會自動使用 Proxy 模式（尤其 Managed Instance 的 public/private endpoint）






#microsoft_interview
