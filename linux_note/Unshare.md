用以實作當前process的namespace


clone创建全新的Namespace，由clone创建的新进程就位于这个新的namespace里。创建时传入 flags参数，可选值有 CLONE_NEWIPC, CLONE_NEWNET, CLONE_NEWNS, CLONE_NEWPID, CLONE_NEWUTS, CLONE_NEWUSER， 分别对应上面六种namespace。

unshare为已有进程创建新的namespace。

setns把某个进程放在已有的某个namespace里。



查看當前user process的namespace訊息
```
$ ls -al /proc/self/ns
```


隔離UTS, hostname
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

隔離PID
```
$ sudo unshare --pid --fork sh 
$ echo $$
1

#ps仍會取到host機器的資訊
#因為ps命令讀取的仍是alp的 /proc目錄
$ mount proc /proc -t proc

```



在許多類 Unix 電腦系統中， procfs 是 行程 檔案系統 (process file system) 的縮寫，包含一個偽檔案系統（啟動時動態生成的檔案系統），用於存取 核心行程資訊。這個檔案系統通常被掛載到 /proc 目錄。由於 /proc 不是一個真正的檔案系統，它也就不占用儲存空間，只是占用有限的記憶體。

proc 還有另一個功能，我們利用它實現 Linux 核心空間與使用者空間 的通訊。在 proc 檔案系統中，我們可以將對虛擬檔案的讀寫作為與核心中實體進行通訊的一種手段，但是與普通檔案不同的是，這些虛擬檔案的內容都是動態建立的。這個虛擬檔案系統會掛載在 /proc 目錄, 而 ps 命令一定會讀取 /proc 目錄, 來顯示目前系統所有 process 資訊