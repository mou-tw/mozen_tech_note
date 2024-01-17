## prerequisite
[[Linux Note Unorganized]]
[[Linux Process]]


## 核心原理

軟體容器化的原理，是透過要把執行的process，和他的相依檔(/bin/apis)
使用namespace做隔離，並整合 Linux 核心功能 chroot/overlay2/CGroup/Virtual Bridge 及 Linux Kernel 虛擬出容器化的概念

**查詢/bin/apis的相依檔[[functional CLI#ldd（List Dynamic Dependenciest)]]**

1. Namespace - 用來提供軟體程序不同的隔離功能
2. CGroup - 用來分配硬體資源
3. Chroot - 針對指定的軟體程序，改變它的根目錄
4. Bridge/slirp4netns - 建立虛擬網路, 連接 Internet 
5. Overlay2 - 用來建立 Container 的檔案系統層



