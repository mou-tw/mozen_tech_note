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
vector<short> v3(5); 有五個長度的vector，內容為0
vector<short> v3(5, 100);
```



