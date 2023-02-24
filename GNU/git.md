### 删除文件
```Shell
git rm xxx.xx
git commit -m "删除了文件xxx.xx"
git push <仓库名/仓库别名> <分支名>
```

### 修改（新建）文件
```Shell
# 当然这个也可以是文件夹，也可以是 .
git add XXXXXXXXX
git commit -m "修改了xxx"
git push <仓库名/仓库别名> <分支名>
```

### 仓库别名
```Shell
git remote  -v // 查看远程仓
git remote rename trunk(现有远程仓别名)  private(自定义的远程仓别名) //  修改远程仓别名
git remote  // 将会显示 master(默认远程主仓别名) 和 private (自定义的个人仓别名)
git remote private // 删除远程仓
git remote // 只会显示master(默认远程主仓别名)
git remote add <别名> <仓库地址> 
```
