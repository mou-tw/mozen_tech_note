在array的使用中，有許多限制，如超過長度沒有安全報警，且每次遍歷都需計算長度，在c++文在另一種模板的vector(類似python 的list，但限制類型)


需要引入library vector
```
#include<vector>
using namespace std;
```

定義vector
```
//empty vector
vector<int> v1; 
//copy 初始化
vector<char> v2 = {'a','b', 'c'};
vector<char> v2{'a','b', 'c'}; //同上

//直接初始化
vector<short> v3(5); 有五個長度的vector，個別值為0
vector<short> v3(5, 100); 有五個長度的vector，個別值為100
```

indexing vector
```
vector<char> v2 = {'a','b', 'c'};
cout << v2[0] //OK
cout << v2[5] //error
```

iterating vector
```
//獲取vector siz

vector<char> v2 = {'a','b', 'c'};
cout<< v2.size();

for (int i =0; i < v2.size(); i ++){
	cout << v2[i];
}

for (int num : v5){
	cout << num;
}
```


新增vector元素
```
vector<char> v2 = {'a','b', 'c'};

v2.push()

```