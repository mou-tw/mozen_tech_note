enum類型的特色是，與struct不同，內容全都是常量

declare
```
enum en {
	Mon, Tue, Wed, Thu, Fri, Sun
}

en en1 = Mon;
cout << en1; // 0

en en2 = 3

```