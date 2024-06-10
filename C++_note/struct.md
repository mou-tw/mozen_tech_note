現實中開發，需要將很多數據類型組合在一起表示，此功能類似python 的list，struct就是用戶自定義的複合式數據結構

declare
```
struct <name>
{
	類型1 數據對象1;
	類型2 數據對象2;
}

ex:
struct PeopleInfo{
	string name;
	int age;
	int sex;
}

PeopleInfo people = {"Jack', 22, 1};
PeopleInfo people{"Jack', 22, 1};//等價上方語法

```