OCI所定義的image，本質上是rootfs加上相關需求模組、套件的目錄壓縮檔，以及相關參數檔的結構體。
OCI image幾個組件概念
- manifest
- image index(optional)
- file system layers
- configuration

檢視 OCI Image 結構順序 
Image Index -> Image Manifest ->  Image Configuration + Filesystem Layer
透過OCI image，並unpack解壓縮打開後，最重要的關鍵是取得bundle(Runtime File System Bundle)
再透過bundle，OCI runtime可建立image

#### skopeo
可使用skopeo這個工具，對image來做操作，不需要透過docker或者podman，運行也不需要透過daemon，同時也是rootless的工具

安裝skopeo
因目前ubuntu不支援，需安裝其他OS，以alpine liunx為例
```
#alpine linux
$ docker run --rm -it --name alp alpine

# in docker container
$ apk add skopeo
$ skopeo -v

#檢查docker hub 上的python images
$ skopeo list-tags docker://docker.io/python
$ skopeo inspect docker://docker.io/python

```

skopeo copy
skopeo 可以從image倉庫中複製image，只要是符合OCI的distribution specification的倉庫都可以拉取(quay, dockerhub, openshift, GCR.....)，也可以從本地容器儲存後端(docker, podman )的image layout目錄等

### 不使用docker，將image下載至docker 本地後端倉庫
```
$ skopeo copy --insecure-policy docker://busybox:latest docker-archive:/tmp/busybox.tar
docker-archive儲存後的文件，可被docker load 讀取

同理可將image從docker 本地倉庫複製到本地其他目錄
$ skopeo copy docker-archive:/tmp/busybox.tar docker-daemon:busybox:latest
$ docker images
```

將image下載到指定目錄
```
$ skopeo copy --insecure-policy docker://busybox:latest dir:/tmp/busybox
```

將image以OCI 規範下載到指定目錄
```
$ skopeo copy --insecure-policy docker://busybox:latest oci:/tmp/busybox
$ tree /tmp
index.json基本存放該image的訊息，包括manifest、config和壓縮後的rootfs檔案



```

blob- binary large object

[ref](https://lework.github.io/2020/04/13/skopeo/)

### 檢視OCI format的資訊
```
$ cat /tmp/busybox/oci-layout
$ cat /tmp/busybox/index.json | jq
{
  "schemaVersion": 2,
  "manifests": [
    {
      "mediaType": "application/vnd.oci.image.manifest.v1+json",
      "digest": "sha256:538721340ded10875f4710cad688c70e5d0ecb4dcd5e7d0c161f301f36f79414",
      "size": 858
    }
  ]
}

#查詢OCI specification中的manifest檔案
$  cat  /tmp/busybox2/blobs/sha256/538721340ded10875f4710cad688c70e5d0ecb4dcd5e7d0c161f301f36f79414 | jq


#觀察config檔案
$ cat  /tmp/busybox2/blobs/sha256/538721340ded10875f4710cad688c70e5d0ecb4dcd5e7d0c161f301f36f79414 | jq

#解壓縮file layer
$ mkdir /tmp/busybox-fs
$ tar xvfz /tmp/busybox2/blobs/sha256/9ad63333ebc97e32b987ae66aa3cff81300e4c2e6d2f2395cef8a3ae18b249fe  -C /tmp/busybox-fs/
$ tree /tmp/busybox-fs/
```


### 使用umoci unpack image
```
#查看image版本
$ umoci ls --layout <oci format image dir> 
# 查看image狀態
$ umoci stat --image /busybox:version

# 創造new image tag
$ umoci tag --image /busybox:1.0 tmp
$ umoci ls --layout <oci format image dir> 

# delete image tag 
$ umoci rm --image /busybox:tmp


# unpack image into bundle
$ umoci unpack --image <oci format image dir> <bundle path>
$ umoci unpack --image /busybox:1.0 /tmp/busybox-bundle
$ ls -al /tmp/busybox-bundle
$ tree /tmp/busybox-bundle

```


RUNC
runc 是根據OCI image specification，以GO lang完成的OCI runtimes
因為runc 的API接口是符合OCI 規範的，因此其他OCI rumtime的同樣API接口功能都是相通的
runc 除了create container，也包含linux namespace，chroot，控管cgroup等權限
runc 同時也是docker完成且運作在docker中的OCI runtimes

```
$ 

```




















