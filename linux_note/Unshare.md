用以實作當前process的namespace


clone创建全新的Namespace，由clone创建的新进程就位于这个新的namespace里。创建时传入 flags参数，可选值有 CLONE_NEWIPC, CLONE_NEWNET, CLONE_NEWNS, CLONE_NEWPID, CLONE_NEWUTS, CLONE_NEWUSER， 分别对应上面六种namespace。

unshare为已有进程创建新的namespace。

setns把某个进程放在已有的某个namespace里。


隔離UTS, hostname
```
$ 
```

