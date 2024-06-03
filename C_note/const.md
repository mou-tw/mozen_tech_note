const修飾過後的變數，不能再被賦值，read only
```
char strA[] = "test"
char *strB = "test"
const char *strC = "test"

strA[0] = 'T' // OK
strB[0] = 'T' // undefined
strC[0] = 'T' // 失敗，因strC為const



```

