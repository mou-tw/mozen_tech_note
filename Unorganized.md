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
![[Pasted image 20240115184607.png]]