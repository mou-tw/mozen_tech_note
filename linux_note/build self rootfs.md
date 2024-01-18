container其中一個核心的概念就是使用chroot更換root的位置
[[chroot#在chroot 的root folder 安裝busybox]]

### 使用ubuntu rootfs
```
$ mkdir -p /myrootfs/rootfs
$ cd /myrootfs
$ wget http://cdimage.ubuntu.com/ubuntu-base/releases/20.04.3/release/ubuntu-base-20.04.3-base-arm64.tar.gz
$ tar xzvf ubuntu-base-20.04.3-base-arm64.tar.gz  -C rootfs/
$ chroot rootfs /bin/bash

```

Q: rootfs能否adduser
> yes, 因為rootfs 幫我們準備好所需的目錄, 檔案及權限設定

Q: /dev 和/proc中是否有訊息
>minirootfs預設沒有host機上的硬體，等同/dev中是空的，預設/proc中也是空
/proc中的資料全部都存在memory，是一種記憶體型的虛擬目錄
ps命令本身也是去讀取proc




other linux os rootfs
[alpine](http://dl-cdn.alpinelinux.org/alpine/v3.19/releases/x86_64/alpine-minirootfs-3.19.0-x86_64.tar.gz)


