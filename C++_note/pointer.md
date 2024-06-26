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


## pointer with const
pointer和const的混用，有兩種狀況
1. 指向常量的指針
   即指針指向一個常量，不是常量無法賦值。
```
const int c1 = 1234, c2 =2345;
int* pc = &c1 //失敗，因為此時的pc並不是const
const int* pc = &c1;
*pc  = 15 //錯誤
pc = &c2 //成功將pc指向到c2

void func(int* const pc){}

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


## array pointer
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


## pointer array & array pointer
- pointer array - 一個array中全部都是相同類型的pointer
- array pointer - 指向一個array的pointer
```
int arr[] = {1,2,3,4,5};
int* pa[5]; //pointer array
int(* ap)[5]; // array pointer

pa[0] = arr;
pa[1] = arr + 1;
pa[2] = arr + 2;

cout << pa[0] << endl; // 0x61fe00
cout << pa[1] << endl; // 0x61fe04
cout << pa[2] << endl; // 0x61fe08
cout << *pa[2] << endl; //3
cout << &arr << endl;  // 0x61fe00
cout << arr << endl;  // 0x61fe00

ap = &arr;
cout << ap << endl;  // 0x61fe00
cout << *ap << endl; // 0x61fe00
cout << **ap << endl; // 1

*(*ap + 1 ) = 100;
for (int n: arr){
	cout << n << " ";
} // 1 100 3 4 5

ap = arr;// 無法賦值，因為arr是變量名，而ap本身是一個指向一個有五個元素的array地址
```



## Pointer應用案例
reverse array
不使用pointer
```
#include<iostream>

using namespace std;

int main(){
	const int n =4
	int orgAry[n] = {1,2,3,4};
	int newAry[n];
	for (int i=0; i <n; i++){
		newAry[n-i-1] = orgAry[i]
	}
	for (int num: newAry){
		cout << num << "\t";
	}
}

```

double pointer
```
#include<iostream>

using namespace std;

int main(){
	const int n =4;
	int orgAry[n] = {1,2,3,4};
	int head = 0, tail = n-1;
	while (head < tail){
		int temp = orgAry[head];
		orgAry[head] = orgAry[tail];
		orgAry[tail] = temp;
		++head;
		--tail;
	}

	for (int num: orgAry){
		cout << num << "\t";
	}
}

```


## function return pointer
在需要return array的場景下，因為array無法複製，因此需要改為return pointer，其中比較簡易的方式為使用typedef
```
//傳統寫法
int(*fun(int x))[5];

//簡化函數聲明
typedef int aryT[5]; 
//aryT是一個自定義的類型別名，表示長度為5的int array


//尾置返回類型
auto func3(int x) -> int(*) [5];

```


## function pointer
聲明指向function 的pointer，需在原先函數名稱的位置上填上*
且不需要聲明型參名稱。
```
string(* fp)       (string, int, int)
      pointer name  (型參列表)

ex: 

string demo1(string s, int i1, int i2){
	.......
}

string(*fp) (string, int, int) = nullptr;
fp = &demo1;
fp = demo1; //可不加取址符
(*fp)("a", 1, 1) //等同調用function
fp("a", 1, 1) //也可調用function


```
有了function pointer，就能夠解決function本身不能夠做為型參傳入的問題。
```
typedef decltype(funcname) *typefunc2

void demofunc(const string&, typefunc2)
```

## return function pointer
function 同樣不能作為值回傳，僅能return pointer
```
typedef decltype(funcname) *typefunc2

//傳統定義方式
typefunc2 func3(int);

//使用尾置方式定義
auto func3(int) -> typefunc2
```