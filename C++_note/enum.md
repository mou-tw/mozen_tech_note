enum類型的特色是，與struct不同，內容全都是常量

declare
```
enum en {
	Mon, Tue, Wed, Thu, Fri, Sun
}

en en1 = Mon;
cout << en1; // 0

en en2 = 3 //錯誤
en en3 = en(3)
cout << en3;// 3


//也可強制指定枚舉量
enum en {
	Mon, Tue, Wed = 99, Thu, Fri, Sun
}

en en4 = Wed;
en en5 = Thu;
cout << en4;// 99
cout << en5;// 100

```