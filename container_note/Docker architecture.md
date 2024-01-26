![[Pasted image 20240124161314.png]]

[ref](https://tarangsharma.hashnode.dev/docker-engine-architecture)

沒有任何 Container 執行時, 只有 dockerd 及 containerd 這二個 Daemon 在運作
```
$ ps aux | grep -v grep | grep dockerd
$ ps aux | grep -v grep | grep containerd
$ docker run --rm -d quay.io/cloudwalker/alpine sleep 120
$ mount | grep overlay
$ ps aux | grep -v grep | grep 'containerd-shim'

```



docker driver
會自動把image 解包打開，把壓縮檔掛載overlay2 的lowwer dir


