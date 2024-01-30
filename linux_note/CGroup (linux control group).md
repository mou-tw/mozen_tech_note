cgroup 是Linux kernal中的功能，用以限制、記錄並且隔離host機器的資源
透​​​過​​​使​​​用​​​ cgroup，系​​​統​​​管​​​理​​​員​​​能​​​取​​​得​​​分​​​配​​​、​​​處​​​理​​​優​​​先​​​順​​​序​​​、​​​拒​​​絕​​​、​​​管​​​理​​​，以​​​及​​​監​​​控​​​系​​​統​​​資​​​源​​​的​​​細​​​部​​​控​​​制​​​。​​​硬​​​體​​​資​​​源​​​可​​​機​​​敏​​​地​​​分​​​配​​​於​​​工​​​作​​​與​​​使​​​用​​​者​​​之​​​間​​​，並​​​提​​​昇​​​整​​​體​​​效​​​率​​​。​​​

cgroup本身是針對process的資源限制，而非用戶

cgroup 的controller範圍很廣，從cpu, memory到部分的硬體周邊
>cpuset-設定使用哪幾個cpu core
>cpu - 設定cpu share,數值越高能用cpu的比例越多
>memory- 使用的memory量
>bikio - 限制如磁碟或usb的輸入輸出控制
>devices - 允許或拒絕對設備的訪問
>freezer- 暫停和恢復cgroup任務
>net_cls- 標記cgroup中process的網路封包，可以再透過tc(traffic control)模塊對數據包進行控制
>net_prio- 設計網路流量的優先程度
>ns - 使不同cgroup的process使用不同的namespace
>hugetlb- 針對hugerTLB系統進行限制

查看linux 的cgroup版本
```
$ stat -fc %T /sys/fs/cgroup/

For cgroup v2, the output is cgroup2fs.
For cgroup v1, the output is tmpfs.
```



## hands on cgroup v2
```
$ stat -fc %T /sys/fs/cgroup/
cgroup2fs

$ ls -al /sys/fs/cgroup/

# 確認目前 Cgroup 有支援那些 運算資源 限制
$ cat /sys/fs/cgroup/cgroup.subtree_control
cpuset cpu io memory hugetlb pids
#建立 的 Example 目錄中, 會繼承 /sys/fs/cgroup/cgroup.subtree_control 宣告的 控制項目 
$ sudo ls -al /sys/fs/cgroup/Example/ | grep cpu
Enable the CPU-related controllers in /sys/fs/cgroup/Example/ to obtain controllers that are relevant only to CPU:
$ echo "+cpu" | sudo tee /sys/fs/cgroup/Example/cgroup.subtree_control
+cpu
$ echo "+cpuset" | sudo tee /sys/fs/cgroup/Example/cgroup.subtree_control
+cpuset
$ sudo mkdir /sys/fs/cgroup/Example/tasks/

$ ls -al /sys/fs/cgroup/Example/tasks/

Ensure the processes that you want to control for CPU time compete on the same CPU:
$ echo "1" | sudo tee /sys/fs/cgroup/Example/tasks/cpuset.cpus

This command sets CPU time distribution controls so that all processes collectively in the /sys/fs/cgroup/Example/tasks child group can run on the CPU for only 0.2 seconds of every 1 second.
$ echo "200000 1000000" | sudo tee /sys/fs/cgroup/Example/tasks/cpu.max


$ yes &>/dev/null &
$ echo "20434" | sudo tee /sys/fs/cgroup/Example/tasks/cgroup.procs
$ top

```


## hands on cgroup v1
v1的 cgroup操作需要安裝額外工具
```
#沒有要額外安裝
#ubuntu預設已安裝
$ sudo apk add cgroup-tools --update-cache --repository http://dl-3.alpinelinux.org/alpine/edge/community/ --allow-untrusted

自定 cpu 控制群組
$ sudo cgcreate -g cpu:/low; sudo cgcreate -g cpu:/high
$ sudo cgset -r cpu.shares=512 low; sudo cgset -r cpu.shares=2048 high


```