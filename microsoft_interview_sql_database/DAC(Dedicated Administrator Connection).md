## 什麼是 DAC？

- **DAC** 是一種專門為資料庫管理員設計的應急連線機制，用於當 SQL Server 或 Azure SQL 在資源瓶頸、高負載或不可響應時，仍能登入並執行基本排錯工作
- 它預留獨立工作執行緒與記憶體資源，即使伺服器資源耗盡，也能優先處理 DAC 連線 。

## 使用限制與權限

- 只有 sysadmin 角色的帳號可使用 DAC
- 每個實例僅能同時存在 一個 DAC，多重連線會被拒絕
- 支援平行查詢、重複需高資源的命令（如 Restore、Backup），僅限做簡單查詢與診斷

## 連線場景
- 本機連線
  預設只允許來自本機的 DAC 連線（127.0.0.1），透過 sqlcmd -S 127.0.0.1,1434 -A 或 SSMS 連線
-  遠端連線
  若要從遠端使用 DAC，需在本機執行：
```
sp_configure 'remote admin connections', 1;
RECONFIGURE;
```
啟用後，DAC 會在 TCP 1434 或啟動時指定的其他動態埠上監

## 應用場景
- 當出現資源耗盡、連線被拒、或工作者(worker)達上限（如 Error 10928）時，DAC 可繞過通常限制建立診斷連線
- 連線成功後，可執行 DMV 查詢，例如 `sys.dm_exec_sessions`, `sys.dm_exec_requests`, `sys.dm_tran_locks`，找出瓶頸與長時間查詢，必要時可使用 `KILL` 終止 SESSION
  [[deadlock and blocking]]



#microsoft_interview
