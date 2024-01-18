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

```