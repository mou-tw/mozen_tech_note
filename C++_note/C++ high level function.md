C++相較傳統C語言，提供很多新的函數特徵

## Inline function
為提高code的重用性和模組化功能，新增了inine functin，同時也能增快運行速度，舉例而言，每次調用函數，會需要在code之間跳轉和調用，為解決這個問題，使用Inline function，會在調用點直接展開，即使用函數code內容代替調用function。


## default parameters
可設定function的默認參數值，和python相同，默認參數值必需後置。
```
int demo(int demo = 111){
	....
}

```

## function reload 
C++在同一個作用