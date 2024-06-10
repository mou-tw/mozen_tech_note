### typein 讀取 

內建有iostream的library
但在每次的輸入流中遇到空白、換行和tab都會結束
舉例而言輸打Hello World，因遇到空白鍵，會被視為結束，僅會捕捉到Hello，需要在定義另一個string捕獲輸入值

```
string s1, s2;
cin >>s1 >>s2;
cout << s1 << s2; //會少一個空白鍵
```

使用getline function，解決空白鍵的問題，以讀取整行string
會讀取string，直到遇到換行符

```
string s3
getline(cin, s3)
```


### 文件IO
ifstream 用於讀取，if 為input filesystem

```
ifstream input("xxx.txt");

//逐字讀取
string word;
while (input>> word){
	cout << word ;
}

//逐行讀取
string line;
while (getline(input, line)){
	cout << line;
	}
	
//逐字符讀取
string ch;
while (input.get(ch)){
	cout << ch;
}

}
```

ofstream 用於輸出寫入文件
```
ofstream output('xxx.txt);

//逐字寫入
string word; 
while (output>> word){
	cout << word ;
}

//逐行寫入
string line;
while (getline(output, line)){
	cout << line;
	
//逐字符輸出
string ch;
while (output.get(ch)){
	cout << ch;
}
```


