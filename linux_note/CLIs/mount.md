linux中的mount有諸多功能
可以透過mount bind兩個不同的資料夾
也可以透過-t指定不同的mount type

```
#創兩個folder，並透過mount指令做綁定
$ mkdir -p {a,b}
$ sudo mount --bind a b
#此時輸入mount語法可查看多了一個ext4的紀錄在b folder
#在a folder此時新增一個資料夾
$ mkdir a/aa; ls a
#查看b foler是否有出現aa
$ ls b
#取消mount
$ sudo umount b
# a/aa 仍存在，而b/aa消失
```


### create overlay2 mount
```
$ mkdir -p ~/overlay2/{lower1,lower2,upper,merged,work}
$ echo 'lower1' > lower1/in_lower1.txt; echo 'lower2' > lower2/in_lower2.txt; echo 'upper' > upper/in_upper.txt; echo 'lower1_both' > lower1/in_both.txt; echo 'lower2_both' > lower2/in_both.txt; echo 'upper_both' > upper/in_both.txt
$ tree
.
├── lower1
│   ├── in_both.txt
│   └── in_lower1.txt
├── lower2
│   ├── in_both.txt
│   └── in_lower2.txt
├── merged
├── upper
│   ├── in_both.txt
│   └── in_upper.txt
└── work



```