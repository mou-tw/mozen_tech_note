```
**

設定 Docker Daemon 自動啟動

$ sudo rc-update add docker boot

  

將 bigred 帳號加入 docker 群組後, 就不需使用 sudo 命令執行 docker

$ sudo addgroup bigred docker

  

重新開機 (一定要執行)

$ sudo  reboot**
```