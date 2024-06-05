C語言中，function和object很類似都有各自名稱
但object可複製，function無法複製
相似之處為都可使用pointer操作
```
int main(){
	void hello(); //宣告一個型態為void()的，名稱為hello的function
	void (*func)() = &hello;
	// 宣告定義func指標，指向一個void()型態的函式
	(*func)() //呼叫func函式

return 0;
}

```