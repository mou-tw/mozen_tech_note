查看當前linux tty和shell程式訊息
```
$ tty
/dev/pts/0
linux dev 資料夾表示設備
pts(pseudo-terminal slave)為虛擬終端

$ echo $SHELL
```


查看當前process訊息
```
$ ps -eo user,pid,ruid,euid,cmd
```
![[Pasted image 20240115191605.png]]
RUID: run程式身分的uid，root為0
EUID(effective UID): 真正提供權限的UID


查看系統中有多少具有SETUID功能
```
$ sudo find / -user root -perm -4000 2>/dev/null | grep -E '^/bin|^/usr/bin'
```

