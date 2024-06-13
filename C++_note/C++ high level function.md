C++相較傳統C語言，提供很多新的函數特徵

## Inline function
為提高code的重用性和模組化功能，新增了inline function，同時也能增快運行速度，舉例而言，每次調用函數，會需要在code之間跳轉和調用，為解決這個問題，使用Inline function，會在調用點直接展開，即使用函數code內容代替調用function。


## default parameters
可設定function的默認參數值，和python相同，默認參數值必需後置。
```
int demo(int demo = 111){
	....
}

```

## function overload 
C++在同一個作用域下，同一個函數名只要型參列表不同，就可以重複定義多次，在調用時根據傳入的類型，決定調用的函數。
```
void printAry(const int* ary, int size){
	...
}


void printAry(const int(&ary)[6]){
	...
}

```

const 在function型參定義位置不同，修飾的意思也不同
```
//修飾型參本身
void func(int* const pc){} 

//修飾型參所指向的參數，或者說預計傳入的參數
//等同間接限制了傳入參數是否為常量
void func2(const int* p)
//func2 與 func3可形成function overload
void func3(int* p)


```