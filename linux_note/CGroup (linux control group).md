cgroup 是Linux kernal中的功能，用以限制、記錄並且隔離host機器的資源
透​​​過​​​使​​​用​​​ cgroup，系​​​統​​​管​​​理​​​員​​​能​​​取​​​得​​​分​​​配​​​、​​​處​​​理​​​優​​​先​​​順​​​序​​​、​​​拒​​​絕​​​、​​​管​​​理​​​，以​​​及​​​監​​​控​​​系​​​統​​​資​​​源​​​的​​​細​​​部​​​控​​​制​​​。​​​硬​​​體​​​資​​​源​​​可​​​機​​​敏​​​地​​​分​​​配​​​於​​​工​​​作​​​與​​​使​​​用​​​者​​​之​​​間​​​，並​​​提​​​昇​​​整​​​體​​​效​​​率​​​。​​​

cgroup本身是針對process的資源限制，而非用戶

cgroup 的controller範圍很廣，從cpu, memory到部分的硬體周邊
>cpuset-設定使用哪幾個cpu core
>cpu - 設定cpu share,數值越高能用cpu的時間越多
>memory- 使用的memory量
>cgroups子系统：

1.blkio 限制每个块设备的输入输出控制。例如:磁盘,光盘以及usb。
2.cpu 限制使用cpu比例
3.cpuacct 产生cgroup任务的cpu资源报告。
4.cpuset 多核心的cpu时为cgroup任务分配单独的cpu和内存 绑定进程和cpu减少上下文切换 内存访问情况 就近访问内存
5.devices 允许或拒绝对设备的访问。
6.freezer 暂停和恢复cgroup任务。
7.memory 设置内存限制以及产生内存资源报告。
8.net_cls 可以标记 cgroups 中进程的网络数据包，然后可以使用 tc 模块（traffic control）对数据包进行控制。
9.net_prio — 这个子系统用来设计网络流量的优先级。

10.ns 可以使不同 cgroups 下面的进程使用不同的 namespace。

11.hugetlb — 这个子系统主要针对于HugeTLB系统进行限制，这是一个大页文件系统。





查看linux 的cgroup版本
```
$ stat -fc %T /sys/fs/cgroup/

For cgroup v2, the output is cgroup2fs.
For cgroup v1, the output is tmpfs.
```



