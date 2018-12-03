# git
## 删除远程分支
查看远程分支
`git branch -r`

删除指令
```
git branch -r -d origin/branch-name
git push origin :branch-name
```

## pull指令
pull的作用就相当于fetch和merge，自动合并：
```
git fetch origin master ///设定当前分支的FETCH_HEAD为远程服务器的master分支
git merge FETCH_HEAD
```
`FETCH_HEAD`指的是：某个branch在服务器上的最新状态。

## push指令
```
git push origin HEAD:branch-name
```
