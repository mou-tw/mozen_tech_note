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


```