C++可以為數據對象另外取一個名子，本質上並不是數據對象，不會占用memory，且宣告同時必須初始化，而宣告對象必須是一個變量。
行為上，refernence 和pointer非常相似，只是pointer的對象能夠修改且具有彈性，refernence是專一指向一個變量。
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


int i = 10;
const int& cref2 = i; //正確
const int& cref3 = 10; //正確
double d = 3.14;
const int& cref4 = d; //正確，double會被轉型為int


```


reference vs pointer const  
```
int a = 10;
int& ref = a;
int* const p = &a;

ref = 20;
cout << a; // a= 20
*p = 30;
cout << a; //a =30

//綁定pointer的refernece
int* ptr = &a;
int*& pref = ptr //

```


引用reference 進行傳參
如果想要利用function進行原始值修改，除了透過傳遞pointer，也可以傳遞reference，避免數據copy。
```

void increase(int& x){
	++x;
}

int main(){
	int n=0;
	increase(n);
}
```