自定義一個類型，可用於解決代碼過長不好閱讀的問題

```
//定義新的類型
//函數類型
typedef const string& typefunc (const string& , const string& )
//函數指針類型
typedef const string& (*typefunc) (const string& , const string& )
//另外的語法
typedef


//typefunc可直接作為型參使用
void demofunc(const string&, typefunc)
```