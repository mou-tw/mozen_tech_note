pointer(type*):為儲存記憶體位置的資料型別
宣告語法:
	在變數名稱前增加*，取得該記憶體的
取址運算址
	&變數，取得

```
int a =1;
int aAddr = &a;
int aa;
aa = *aAddr; 

```