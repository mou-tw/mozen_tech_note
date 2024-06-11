C++可以為數據對象另外取一個名子，本質上並不是數據對象，不會占用memory，且宣告同時必須初始化，而宣告對象必須是一個變量。
```
int a = 10;
int& ref = a; //ref是a的reference

cout << &a ;
cout << &ref ;結果同上

ref = 20;
cout << a ; //a= 20

```


## const reference
對常量的reference
```
const int c = 0;
const int& cref = c;

cref = 10 //錯誤，const不得修改


int 
```
