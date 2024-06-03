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
		print("%d\n", *n) // 此時
	}
}
```