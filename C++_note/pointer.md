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