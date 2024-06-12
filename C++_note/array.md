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


多維array
```
二維數組
int ary[][]
int ary[][] = {1,2,3,4,5,6}
int ary[][] = {{1,2,3}, {4,5,6}} //等價上方array
三維數組 
int ary[][][]
```



遍歷array
```
for (int i =0; i <arrySize; i++){
	cout << ary[i];
}

for (int num : ary){
	count << num


//多維array
int row = size(ary) / sizeof(ary[0])
int col = size(ary) / sizeof(ary[0][0])

for (int i =0; i < row ; i++){
	for(int j =0; j < col; j++){
		cout << ary[i][j];
	}
}
}
```

notes:
- array不能夠直接複製

array傳參
由於array無法複製，必須要改傳遞array的pointer
```
//舉例遍歷array的function
//兩者皆可
//加const是不希望array不被修改

void enuAry(const int arr[]){

}

void enuAry(const int* ptrAry ){
	
}

//因為無法針對pointer計算得知array長度，仍造成一定困難
//解法1，另外傳入array的長度






```





