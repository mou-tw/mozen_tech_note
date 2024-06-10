string和vector類似，針對char的一個長度可控序列
使用需要引入
```
#include<string>
using namespace std;
```

初始化
```
string s1;
//copy初始化
string s2 = s1;
string s3 = "Hello World";
//直接初始化
string s4("Hello World);
string s5(8, "a"); //8個a的string
```

indexing 
```
string s4("Hello World);
cout << s4[0];
S4[0] = "H";
cout << s4
```

size
```
cout << s4.size();
```

string 相加
string長度不定，可透過相加的方式做相加
```
string s1 = "Hello ";
string s2 = "World";
string s3 = s1 + s2;

//錯誤操作，字串符不能直接相加，僅有string可以
strig s4 = "hello"  + "world"
```


C style string

需要有\0結束符
```
char c1[5] = {"H", "E", "L", "L", "O"};    //not sting
char c2[6] = {"H", "E", "L", "L", "O", "\0}; //string

cout << c1 //會print出很多未定義的數據(Oxcc)的表示值，值到遇到\0
cout << c2 //正常
```