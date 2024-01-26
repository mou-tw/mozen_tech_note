[OCI(Open Container Initiative)](https://opencontainers.org/)
OCI為制定容器標準的組織，定義了容器的規格以及API調用接口規格，任何組織只要遵循這個規範，都可以發布自己的OCI rumtime以及image registry，OCI標準主要有三個規格
- Image - 定義container的結構
- Distribution- 定義image如何被儲存以及取用(repo, graphic DB)
- Runtime- 具體定義contianer如何被創建，runC, crun, runsc, KATA
> runc >> docker 使用go lang開發
crun >> google使用C語言開發
runsc >>  google設計run security，有自己的kernal，
[kata](https://katacontainers.io/) >> kata，container具有VM的安全性


[ref linux KVM](https://aws.amazon.com/tw/what-is/kvm/)


![[Pasted image 20240124205902.png]]
- container檔案系統肯定為overlay2
- image中的layers是很多tar.gz的壓縮檔[[OCI Image Techniques (skopeo, umoci)#檢視OCI format的資訊]]
- rootfs從image unpack而來[[OCI Image Techniques (skopeo, umoci)#使用umoci unpack image]]
- runtime config宣告container電腦名稱要叫甚麼、要跑甚麼process、設定cgroup資源限制,capability等基礎技術的宣告
- 而將runtime config和rootfs合而為一的概念為bundle
- 透過OCI runtime在bundle資料夾底下可運行

而需要的檔案系統在rootfs

將rumtime和rootfs合而為一的概念稱為bundle








container structure

![[Pasted image 20240124210213.png]]