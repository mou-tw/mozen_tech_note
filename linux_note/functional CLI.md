## ldd（List Dynamic Dependenciest)
查詢/bin底下CLI的相依檔案
```
ldd /bin/ls
	/lib/ld-musl-x86_64.so.1 (0x7fa636bcc000)
	libc.musl-x86_64.so.1 => /lib/ld-musl-x86_64.so.1 (0x7fa636bcc000)
```

