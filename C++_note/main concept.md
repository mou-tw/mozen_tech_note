C++由C開發而來
C是一種面向過程(POP)的語言，缺點是不適合大型的專案開發
C++是物件導向(OOP)的語言，包含class和object的實踐以及操作方法

C++的source code檔名為.cpp，目標代碼為.obj文件，經鏈接引入library文件後的可執行代碼為.exe

## 數據類型
int




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

