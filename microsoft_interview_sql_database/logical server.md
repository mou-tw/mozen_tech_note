Logical Server 是在 Azure SQL Database 和 Azure Synapse 中用來組織資料庫的邏輯容器，不代表實體主機。它不提供 OS 或 instance 存取，僅提供管理接口
在這個 server 下，可統一配置多個 databases 或 dedicated SQL pools 的以下項目：

- 登入帳號 (logins)
- 防火牆規則 (firewall rules)
- 偵測規則 (auditing, threat detection)
- 自動 failover group
- Minimal TLS、Azure AD 設定等
  
為什麼要用 Logical Server？
- **集中管理多個資料庫**  
    可統一管理身份驗證、稽核、安全政策與網路規則，而不必對每個 database 重複操作。
- **命名與連線終點**  
    提供 `<serverName>.database.windows.net` 作為所有 database 的共用入口點，方便 client 應用程式連線與 DNS 設定
- **資源與限制配置**  
  Server 層可控 quota(如 DTU/vCore)、region 區域、elastic pools 線上配置等

|功能項目|Azure SQL Database|Azure Synapse Analytics|
|---|---|---|
|管理 container|✅ 是|✅ 是|
|支援 SQL Database / pools|✅ 是|✅ 包括 dedicated pools|
|Instance-level 功能支援|❌ 否|❌ 否|
|使用場景|OLTP 應用管理|Data warehouse 管理|

## 總結

- **Logical Server** 是一個管理層面上的結構，幫助使用者跨資料庫配置安全、網路、身份、Failover 等功能。
- 它非實體，無 Instance 存取權限，僅相當於一組資料庫的「入口門戶」與安全邊界。
- 適用於 SQL Database 和 Synapse 中管理多資料庫或資料倉情境。
- 若你只使用單一資料庫，可簡化設置，但仍需建立 server 以啟用連線與管理功能。
- 若需要跨資料庫查詢或高兼容性功能，建議使用 **Managed Instance（MI）**，它提供實體 Instance 架構。