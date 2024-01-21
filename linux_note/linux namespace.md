linux每個process都與namespace關聯，且僅能看到與這個namespace相關的resources

kernel version 5.6, there are 8 kinds of namespaces
- Mount (mnt)
- Process ID (pid)
- Network (net)
- Interprocess Communication (ipc)
- UTS (UNIX Time-Sharing)
- User ID (user)
- Control group (cgroup) Namespace
- Time Namespace


| namespace | 作用 |
| :-----| ----: | 
| IPC | 隔離 System V IPC 和 POSIX 消息隊列 |
| network | 隔離網路 |
| mount | 隔離掛載點 |
| PID|隔離PID |
| UTS|隔離主機和域名 |
|user | 隔離用戶|


查看process相關的namespace訊息
```
# 查看當前user scope的namespace
$ lsns 
$ sudo lsns
$ ls -l /proc/<any pid / self>/ns
```




[ref](https://www.jianshu.com/p/ab423c3db59d)
[ref](https://moelove.info/2021/12/10/%E6%90%9E%E6%87%82%E5%AE%B9%E5%99%A8%E6%8A%80%E6%9C%AF%E7%9A%84%E5%9F%BA%E7%9F%B3-namespace-%E4%B8%8A/#contents:cgroup-namespaces)





