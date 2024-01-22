用以實作當前process的namespace


clone创建全新的Namespace，由clone创建的新进程就位于这个新的namespace里。创建时传入 flags参数，可选值有 CLONE_NEWIPC, CLONE_NEWNET, CLONE_NEWNS, CLONE_NEWPID, CLONE_NEWUTS, CLONE_NEWUSER， 分别对应上面六种namespace。

unshare为已有进程创建新的namespace。

setns把某个进程放在已有的某个namespace里。



查看當前user process的namespace訊息
```
$ ls -al /proc/self/ns
```


### 隔離UTS, hostname
```
$ sudo unshare --uts sh 
再查看當前的namespace狀態
$ ls -al /proc/self/ns
會發現uts的namespace不一樣了

#嘗試修改hostname
$ hostname <name>
$ hostname
$ exit 

#測試是否真的有隔離效果
$ hostname
```

### 隔離PID
```
$ sudo unshare --pid --fork sh 
$ echo $$
1

#ps仍會取到host機器的資訊
#因為ps命令讀取的仍是alp的 /proc目錄
$ mount proc /proc -t proc
$ ps 
會發現已看不到原本的process，/proc中的process也發生改變
#取消mount
$ umount /proc
$ exit

## 完整語法
#--mount-proc 這參數, 會將 sh 的 PID Namespace, 掛載到 ALP 的 /proc 目錄
$ sudo unshare --pid --fork --mount-proc sh 
$ sleep 10 $
$ pstree -p
# /proc用下列語法查看，得知被掛載兩次
$ findmnt proc
$ exit
```


在許多類 Unix 電腦系統中， procfs 是 行程 檔案系統 (process file system) 的縮寫，包含一個偽檔案系統（啟動時動態生成的檔案系統），用於存取 核心行程資訊。這個檔案系統通常被掛載到 /proc 目錄。由於 /proc 不是一個真正的檔案系統，它也就不占用儲存空間，只是占用有限的記憶體。

proc 還有另一個功能，我們利用它實現 Linux 核心空間與使用者空間 的通訊。在 proc 檔案系統中，我們可以將對虛擬檔案的讀寫作為與核心中實體進行通訊的一種手段，但是與普通檔案不同的是，這些虛擬檔案的內容都是動態建立的。這個虛擬檔案系統會掛載在 /proc 目錄, 而 ps 命令一定會讀取 /proc 目錄, 來顯示目前系統所有 process 資訊




### 隔離IP
```
$ sudo unshare --net sh 
$ ip a
發現少了一張網路卡
```

實作兩個namespace之間的通訊

| 順序 | terminal1 | terminal2 |
| :--- | ---- | ---- |
| 1 | sudo unshare --pid --fork --mount-proc --net --uts sh |  |
| 2 |  |  sudo lsns -t net |
| 3 |  | sudo ip link |
| 4 |  |  |
| 5 |  |  |
| 6 |  |  |
| 7 |  |  |
| 8 |  |  |
| 9 |  |  |
| 10 |  |  |




| 順序 | terminal1 | terminal2 |
| :--- | ---: | |
| IPC | 隔離 System V IPC 和 POSIX 消息隊列 | |
| network | 隔離網路 | |
| mount | 隔離掛載點 | |
| PID | 隔離PID | |
| UTS | 隔離主機和域名 | |
| user | 隔離用戶 | |




















