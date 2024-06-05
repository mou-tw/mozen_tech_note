pointer(type*):為儲存記憶體位置的資料型別
宣告語法:
	在變數名稱前增加*，取得該記憶體紀錄的值
取址運算址
	&變數，取得記憶體起始位置

```
int a =1;
int aAddr = &a;
int aa;
aa = *aAddr; 

```

```
int main(){

    int ary[3][3] = {{1,2,3},{1,2,3},{1,2,3}};

    int *d[3] = {ary[0],ary[1],ary[2]};

    // walk(d,3,3);

    printf("%d ", ary); //6422000

    printf("%d ", *ary); //6422000

    printf("%d ", ary[0]); //6422000

    printf("%d ", ary[1]); //6422012

    printf("%d ", ary[2]); //6422024

    printf("%d ", *&ary);  //6422000

    printf("%d ", **ary);  //1

    printf("%d ", *(ary[1]+4));  //2

    printf("%d ", d);  //6421968

    printf("%d ", *d); //6422000

    return 0;

}
```


EX:
簡單雙數排序
```
#include <stdio.h>

void order_swap(int *a, int *b);


int main(){

    int a, b;

    printf("enter two numbers:");

    scanf("%d%d", &a, &b);

    order_swap(&a, &b);

    printf("%d %d", a, b);

    return 0;

}

void order_swap(int *a, int *b){

    if (*a> *b){

        int tmp = *a;

        *a = *b;

        *b = tmp;

    }

}
```


存取陣列元素
```

int main(){
	int v[5] = {1,2,3,4,5};
	int *n // 宣告一個pointer 
	for (n = v; n! = v+5; n++){
		print("%d\n", *n) // 此時的*n是取值
	}
}
```


下標運算子 []
C語言中的語法糖，不限制運算子中的類型
```
a[b] 等同 *(a+b)

int v[5];
v[0] >>陣列索引，因陣列無法計算，因此會轉型成為v0的位置

int *n = v
n[0] >> *(n+0)
這個案例中v[0]與n[0]是等價的
甚至0[v]等同v[0]

```

使用pointer傳遞陣列或者字串
由於陣列沒辦法傳遞，因此在函式中所傳遞的陣列或者字串，都會自動隱性轉型成為pointer，再透過下標運算子的特性，操作記憶體位置的值。


字串字面常數
```
char strA[] = 'test'
此操作是直接定義了五個記憶體空間，放入't','e','s','t','\0'
strA[0] = 'T' //可成功修改

char *strB = 'test'
此操作為定義了一個記憶體空間，存放字串的記憶體位置，而't','e','s','t','\0'則是唯讀的，不可以修改，當嘗試修改，屬於為定義行為

strA = "Test" //編譯失敗
strB = "Test" //修改成功


```


陣列的指標
```
int v[3] = {1,2,3}
int (*q)[3] = &v
&v 代表v的記憶體位置，並且代表內容裝載一個array，且有三個int
(*q)代表一個pointer，且有三個int，並將&v的記憶體位置賦值 



```

pointer之間的轉型
絕大部分的情形下，指向不同型別之間的pointer無法直接隱性轉型

```
int intVar;
double doubleVar;

int *intPointer = &doubleVar; //未定義行為
double *doublePointer = &inteVar //未定義行為
int **intPointerPointer = &intVar //未定義行為
int **intPointerPointer = &intVar //未定義行為


```

合法的隱性轉型
int <> float (有精度喪失的問題)
陣列可以轉型為pointer
pointer無法轉為陣列
void pointer可與其他型別的pointer互轉
但void pointer無法取值

不合法的隱性轉型

int 轉型double不被保證


### 空指標 NULL pointer
透過檢查指標是不是null 或者0可得知是否有指向