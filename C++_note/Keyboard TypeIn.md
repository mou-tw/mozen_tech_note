內建有iostream的library
但在每次的輸入流中遇到空白、換行和tab都會結束
舉例而言輸打Hello World，因遇到空白鍵，會被視為結束，僅會捕捉到Hello，需要在定義另一個string捕獲輸入值

```
string s1, s2;
cin >>s1 >>s2;
cout << s1 << s2; //會少一個空白鍵
```

使用getline function，解決空白鍵的問題，以讀取整行string
會

```
string s3
getline(cin, s3)
```