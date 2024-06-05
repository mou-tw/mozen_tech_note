因C的string也是另類的陣列
因此透過陣列的角度修改時可透過pointer的方式
```
char strA[3][4] = {"How","are","you"}
char *strB[3]   = {strA[0],strA[1],strA[2]}

```
透過strB的方式，用pointer指向到strA中每個字串的開頭記憶體位置
