# git 上传项目详细步骤

## 初始化

1. git init

2.  git config --local user.name wjl

3. git config --local user.email zghbwjl@163.com

4. git add .

5. git commit -m 'init admin-ui'

6. 可选步骤，切换分支：git branch -M  main

7. git remote add origin git@github.com:jianliangWang/admin-ui.git

8. git push -u origin main

## stash暂存

```git
# 保存当前未commit的代码
git stash

# 保存当前未commit的代码并添加备注
git stash save "备注的内容"

# 列出stash的所有记录
git stash list

# 删除stash的所有记录
git stash clear

# 应用最近一次的stash
git stash apply

# 应用最近一次的stash，随后删除该记录
git stash pop

# 删除最近的一次stash
git stash drop
```

当有多条 stash，可以指定操作stash，首先使用stash list 列出所有记录：

```git
$ git stash list
stash@{0}: WIP on ...
stash@{1}: WIP on ...
stash@{2}: On ...
```

应用第二条记录：

```git
$ git stash apply stash@{1}
```
