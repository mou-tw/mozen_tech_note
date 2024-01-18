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


因為container的process共用hostOS的kernal，避免container把kernal損毀
一旦kernal損毀，其他container由於本質是process，也會一起損毀
需要使用seccomp和capability一同使用保護kernal



### Docker FileSystem
容器化技術在filesystem上使用了一種叫做聯合檔案系統(Union Filesystem)
Linux中的UnionFS本身也有很多種類型
docker 早期使用AUFS，在新版本之後使用overlay2
查詢當前的docker 所使用的FS，可使用docker info查詢

