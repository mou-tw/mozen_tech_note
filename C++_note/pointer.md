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
```
const int c1 = 1234, c2 =2345;
int* pc = &c1 //失敗
```