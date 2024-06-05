因C的string也是另類的陣列
因此透過陣列的角度修改時可透過pointer的方式
```
char strA[3][4] = {"How","are","you"}
char *strB[3]   = {strA[0],strA[1],strA[2]}

```
透過strB的方式，用pointer指向到strA中每個字串的開頭記憶體位置
這個方式使修改strA的陣列字串成為可能
```
strB[2][0] = 'Y'
```

但如果希望將其中一個string修改，會遇到長度的問題
舉例而言，因為strA中限定每個字串的長度為4，無法直接修改賦值
需透過另外創建一個char的方式修改之
```
char strC[5] = "What";
strB[0] = strC;
strB[0][0] = 'w'
```

但此類方式產生幾個問題
第一是原先的how被閒置了
另外如果每次要產生新的string
必須要每次重新定義新的字串