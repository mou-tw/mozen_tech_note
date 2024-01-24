[OCI(Open Container Initiative)](https://opencontainers.org/)
OCI為制定容器標準的組織，定義了容器的規格以及API調用接口規格，任何組織只要遵循這個規範，都可以發布自己的OCI rumtime以及image registry，OCI標準主要有三個規格
- Image - 定義container的結構
- Distribution- 定義image如何被儲存以及取用(repo, graphic DB)
- Runtime- 具體定義contianer如何被創建，runC, crun, runsc, KATA

![[Pasted image 20240124205902.png]]

