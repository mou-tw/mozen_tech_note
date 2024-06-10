C++中的array和C類似，會把相同類型的數據對象在memory中連續且按照順序儲存，並不是一種特定的數據類型。

array的聲明
```
//數據類型  名稱 [ array個數 ]

int ary[10];

//初始化array
int ary[4] = {1,2,3,4};
int ary[] = {1,2,3,4}; //同上

```

arrary的限制
- 可以比定義的數量少，其餘是未定義
- 初始值不能比定義值多

array的訪問
```
/
int ary[] = {1,2,3,4};

cout << ary[1]; //2
a[1] = 1 ;
cout << ary[1]; // 1
cout << ary[10] ; // 會訪問到未定義數值，不會報錯

```

求知array長度
```
int ary[] = {1,2,3,4};
cout << sizeof(ary);
cout << sizeof(ary[0]);
cout << sizeof(ary) / sizeof(ary[0]);
```

遍歷array
```
for (int i =0; i <arrySize; i++){
	cout << ary[i];
}

for (int num : ary){
	count << num
}
```


多維array
```
二維數組
int ary[][]
三維數組 
int ary[][][]
```










