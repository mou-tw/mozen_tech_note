主要功能為更換linux的根目錄位置
以將 /layer1/layer2/layer3 作為root目錄，並運行sh為例
```
$ mkdir -p /layer1/layer2/layer3/bin
$ mkdir -p /layer1/layer2/layer3/lib
$ cp /bin/sh /layer1/layer2/layer3/bin/
# 需先用ldd查詢sh 的相依檔
$ cp /lib/ld-musl-x86_64.so.1 /layer1/layer2/layer3/lib/
$ chroot /layer1/layer2/layer3/ /bin/sh
# root已改變
```


### 在chroot 的root folder 安裝busybox
透過chroot 的新root, 安裝busybox
在上述的環境之下，新增busybox工具至新的root /bin資料夾中
```
$ cp /bin/busybox /layer1/layer2/layer3/bin/
$ /bin/busybox --install -s 
#過程中很可能有路徑不存在的問題，可退出重新mkdir路徑

$ ls -al /bin
# 查看使用busybox做出來的CLI

$ ls -al /proc
# 查看process訊息

```