# git 上传项目详细步骤

## 配置证书

push到远程仓库的时候需要有认证，要使用ssh密钥认证，不要使用用户名密码

这里针对本地有多个证书配置

1. 生成密钥对

   ssh-keygen -t rsa -C “xxxx@xxxx.com”(xxx为上一句输入的邮箱地址)语句，回车之后生成SSH key，后面出现让输入口令的语句，直接按回车即可，这样系统路径下就生成了两个文件：id_rsa和id_rsa.pub

2. 将公钥配置到平台

   ![](E:\github\document\images\git-set-rsakey.jpg)

3. 将私钥配置到本地

   本地可能有多个地址，所以我们需要进行配置，进入到用户.ssh目录

   编辑config配置文件，如果没有该文件自己新建

   ```bash
   Host 10.10.13.111
   PreferredAuthentications publickey
   IdentityFile ~/.ssh/111_rsa
   	
   # host是你的git服务器地址
   # IdentityFile 你刚刚生成的私钥的名字，多个文件所以需要修改名称，然后将私钥复制到对应目录下
   # 如果是多个，将有多个Host和下面的配置
   ```

   

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
