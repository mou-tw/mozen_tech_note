memory的最小尋址單位是byte，所有的數據就是保存在memory中的一連串bytes中，pointer就是指向其中一memory的某一個位置，就可以找到具體的數據。

pointer本身也是一種數據類型，只是保存的是其他對象的memory地址。

定義pointer變量
```
int* p1 //指向的對象是int類型的pointer
long long* p2 //指向的對象是long long類型的pointer
cout << sizeof(p1) //查看p1的memory的佔用長度
cout << sizeof(p2) //查看p2的memory的佔用長度
//p1 和p2大小相同

```

獲取address給pointer賦值
```
int a = 10, b=20;
int* p1;
p1 = &a;
cout << *p1; //印出p1對應address代表的值
*p1 = 11; 修改值
p1 = &b  //p1重指向為b
```

無效指針(野指針)
定義指針後沒有初始化而且賦值，因為指針本身也是一個變量，預設會指向到一個memory地址，如果直接取值，會不知道取到甚麼值，即無效指針。

null pointer
因避免無效指針的狀況，可透過定義一個空指針來解決之
```
int np = nullptr;
np = NULL;
np = 0;
```

void pointer
和null pointer的差別是，null pointer一開始有定義類型，而void pointer代表可指向任意數據對象，也因為此void pointer只能存儲地址，不能取值(因為不知道具體要取多少bytes)
```
int i = 10;
string  s = "Hello";

void* vp = &i;
vp = &i;
vp = &s;
```


指向指針的指針(二級指針)
因為pointer自身也是一個數據對象，同樣也具備address，所以可以讓一個pointer保存之
```
int i = 123;
int* pi = &i;
int** ppi = &pi;

cout << i ; //123
cout << pi ; //i的address
cout << ppi; // pi的address
cout << *pi; //123
cout << *ppi; // i的address
cout << **ppi; // 123
```


pointer with const
pointer和const的混用，有兩種狀況
1. 指向常量的指針
   即指針指向一個常量，不是常量無法賦值。
```
const int c1 = 1234, c2 =2345;
int* pc = &c1 //失敗，因為此時的pc並不是const
const int* pc = &c1;
*pc  = 15 //錯誤
pc = &c2 //成功將pc指向到c2

```
2. 指針常量
   代表pointer本身是一個常量，保存的address不能修改
```
int i = 1234 ;
int* const cpc = &i;
cpc = &a; //失敗
*cpc = 12345; // i被改成12345
```

3. 混合使用
   希望定義一個指向const的pointer，且這個pointer也不能修改
```
const int* const ccpc = &c1;
```


array pointer
```
int arr[] = {1,2,3,4};
cout << arr // arr的address
cout << &arr[0]  //arr[0] 以及arr的address
cout << &arr[1]  //arr[1]的address

int* pia = arr;
cout << *pia ;// 1
*pia = 100 ;//arr的第一個值被改成100
```

pointer 計算
pointer的計算，會根據儲存類型，增減相對應的地址，例如存放int的array，pointer +1 ，會移動四個bytes的address
```
int arr[] = {1,2,3,4};
int* pia = arr;
cout << pia; //arr[0]或者arr的address
cout << (pia+1); //arr[1]的address 
*(pia+1) = 100 //arr[1] 被修改為100
```


pointer array & array pointer
- pointer array - 一個array中全部都是相同類型的pointer
- array pointer - 指向一個array的pointer
```
int arr[] = {1,2,3,4,5};
int* pa[5] ; //pointer array
int(* ap)[5] ; array pointer



ap = arr;// 無法賦值，因為arr是變量名，而ap本身是一個指向一個有五個元素的array地址
ap = &arr;

```