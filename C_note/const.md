const修飾過後的變數，不能再被賦值，read only
```
char strA[] = "test"
char *strB = "test"
const char *strC = "test"

strA[0] = 'T' // OK
strB[0] = 'T' // undefined
strC[0] = 'T' // 失敗，因strC為const

strA = strB //失敗，因為陣列無法直接被賦值
strA = strC //失敗，因為陣列無法直接被賦值
strB = strA // OK，strA會自動轉型成pointer
strB = strC //失敗，因為strB是可修改的pointer
strC = strA //OK， 陣列會被轉成pointer，再轉成 const pointer
strC = strB //OK， 陣列會被轉成pointer，再轉成 const pointer


```

