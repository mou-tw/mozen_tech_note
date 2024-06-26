C++由C開發而來
C是一種面向過程(POP)的語言，缺點是不適合大型的專案開發
C++是物件導向(OOP)的語言，包含class和object的實踐以及操作方法

C++的source code檔名為.cpp，目標代碼為.obj文件，經鏈接引入library文件後的可執行代碼為.exe

## 數據類型
算術類型
電腦每一次尋址的單位是一個Byte(8bit)，僅能表示256個數字完全不足
因此C++可根據不同尺寸存/讀不同單位的Byte

| 類型        | 含意   | 最小尺寸   |
| :-------- | ---- | ------ |
| bool      | bool | 未定義    |
| char      | 字符   | 8 bit  |
| short     | 短整數型 | 16 bit |
| int       | 整數型  | 16 bit |
| long      | 長整數型 | 32 bit |
| long long | 長整數型 | 64 bit |
查看實作定義的資料長度
```
int a = 1;
cout << sizeof a <<endl;
```

資料型態的算術溢出
由於每個資料型態能夠表達的數字上限有限，如果超過就會產生算數溢出，即退回到最小表示值重新往上加

無符號整數
不希望有負數的情形，可在定義時加入unsigned
```
unsigined int a ;
```

符點數
float 佔32bits(4bytes)
double 佔 64 bits(8 bytes)



## 類型轉換

整數值賦給bool變量
```
bool btrans = 25; // bool為1，true
```
布爾類型賦值給算術整型
```
short strans = false; // trans =0
```
浮點數賦值給整數類型
```
int a = 3.14 // a =3
```
整數賦值給浮點數
```
flaot a = 3; // a = 3.0
```
賦值超出整型範圍
```
unsigned short ustrans = 65536 //ustrans =0
```

## 變量與常量
變量可做修改
常量不可修改
```
//定義變量
int a = 1;
a = 2

//定義常量
//宏定義，可不須聲明data type，名稱通常為大寫
#define B1 123

//一般常量
//常量的職必須初始化
const int b2 = 1


```



流程控制
## 函數
調用其他cpp文件的函數
```
include "demo.cpp"

#定義函式
void demo();
int main(){
	demo(); //呼叫demo.cpp中的demo function
}

```


object and class
memory
library
error handler



```
#include <iostream>

int main()
{
    std::cout << "Hello World" << std::endl;
}
```

std是namespace，
::為作用域運算符，表示左方是namespace
<< 是引導符，等同將hello world輸出到out這個
return 0可省略，因為正常執行就是return 0



## 遞增遞減運算符

```
//會賦值
int i =0, j;
j=++i ; //j=1 i==1
//不會賦值
int i =0, j;
j=i++ ; //j=0 i=1

```

## 條件運算符

<判斷式> ? 如果真: 如果假
等同if else
```
cout<< (1<2 && ++1) ? "true" : "false" << end
```

## 移位運算符
[可操作到底層的bit數據](https://www.youtube.com/watch?v=-Z61t6n3akQ&list=PLmOn9nNkQxJFgVZJqpMCAOtPlExvpZjpk&index=26)
```
<< 左移運算符
>> 右移運算符
```

## 位邏輯運算符
[link](https://www.youtube.com/watch?v=9AhjS_Kz42s&list=PLmOn9nNkQxJFgVZJqpMCAOtPlExvpZjpk&index=27)



## 隱式類型轉換
會將長度較小的類型轉換為長度較大的類型，避免精度喪失
如int和float做計算，會將int轉換為float

## 強制類型轉換
```
// C style
(new type) value

//C++ style
new type (value)

//C++ 強制類型轉換
static_cast<new type> (value)
```


## 條件語句
C++提供了If和switch
```
//if

if (條件){
	行為statement
}
else{
	行為statement
}

// else if

if (條件){
	行為statement
}
else if (條件){
	行為statement
}


//switch
如果沒有匹配的case，會執行default，default可不加
swith (表達式){
	case 值1:
		statement
		break;
	case 值2:
		statement
		break;
	default:
		statement
		break;
}

```


## while loop

```
while(條件){
	statement
}


do{
	statement
}while (條件)

```


## for loop
```
for (int i = 0; i < n; i ++){
	statement
}

遍歷一個數組
for (int n: {1,2,3,4,5,6}){
	statement}
```

## continue
和break不同，continue會跳過當次的條件循環


## static object
希望可以延長一個局部變量的生命週期，使其在作用域外依然保留，可在變量前天加static
```
#include<iostream>

using namespace std;

void print(){
    // int a = 0;
    static int a = 0;
    for (int i = 0; i < 5; i++)
    {
        cout << a << endl;
        ++a;
    }
} 

int main(){
    print();
    print();
}

```


.h文件聲明

```
#pragma once
//需聲明pragma once，代表此文件只會編譯一次
```

使用.h文件
```
#include"xxx.h"
```

 