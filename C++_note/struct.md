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

取值&賦值
```
PeopleInfo people = {"Jack', 22, 1};
cout << people.name
people.name = 'Andy'
```

結構體數組
```
PeopleInfo p[3] = {
	{"Jack', 22, 1},
	{"Andy', 18, 1},
	{"Jydy', 35, 0}
}
cout << p[0].name // Jack
```

遍歷
```
for (PeopleInfo peo: p){
	cout << peo
}
```

