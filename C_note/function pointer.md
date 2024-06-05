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

由於C語言中的function，都是透過pointer呼叫操作的
如下列都可執行成功
```
(&printf)("Hello);
printf會自動轉向printf的function pointer
(*printf)("Hello);
printf會自動轉型為&printf與*取值對消，等同使用printf function
```

函式之間傳遞函式
```


```