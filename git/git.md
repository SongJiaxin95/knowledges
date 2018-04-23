# GIT使用指南
**基础操作**
1. 克隆github或者码云上代码到本地
```
git clone 分支名仓库名地址
```
2. 创建自己的分支
```
git checkout -b 分支名（该语句创建后直接切入该分支）
或
git branch 分支名
```
3. 查看当前修改文件的状态
```
git status
```
4. 添加要上传的文件到仓库/缓存
```
git add 修改后的文件
或
git add . （添加所有文件）
```
5. 提交仓库/缓存文件到本地
```
git commit -m '注解'
```
6. 合并添加上传文件和提交文件到本地的操作
```
git commit -am '注解'
```
>注意：合并语句的前提是被修改文件已经是tracked，就是要先使用一次add添加，然后被修改文件就可以使用合并语句了。
7. 文件到本地分支更新到远程云上
```
git push origin 分支名
```
>说明：第一次提交会让你输入用户名和密码，可以使用以下命令就不用每次都进行输入了
```
git config --global user.name '用户名'
git config --global user.email '邮箱'
```
建议使用SSH密钥，使用方式如下：
```
ssh-keygen -t rsa -C '邮箱'

```
然后找到C:\Users\Administrator\.ssh目录下的id_rsa.pub文件，复制文本内容到云的ssh密钥设置里
以后就不用输入密码了。
8. 下拉远程自己分支代码到本地自己分支
```
git pull origin 分支名
```
---
**代码分支合并，tag提交**
9. 将自己分支代码合并到测试分支以便测试人员测试 先切换版本到dev分支
```
git checkout dev
```
当前dev分支在合并jason分支
```
git merge jason
```
提交dev分支合并的代码到远程dev分支上
```
git push origin dev
```
10. 上线代码需要打tag，在master分支打tag 打版本v1.0.0.0
```
git tag -a 版本号 -m '注解'
```
提交版本
```
git push origin 版本号
```
---
**分支版本处理**
11. 删除本地分支
```
git branch -D jason
```
12. 删除git远程分支
```
git push origin --delete jason
```
13. 删除本地tag
```
git tag -d v1.0.0.0
```
14. 删除git远程tag
```
git push origin --delete tag v1.0.0.0
```
15. 查看dev分支和jason分支的不同
```
git diff dev wanghaifei
```
---
**缓存机制，在某一个分支修改了代码，但是不想提交该分支，又想切换到另外一个分支在修改相同的代码，就需要使用stash命令**
16. 缓存本地修改的代码
```
git stash
```
缓存之后，在git status去查看修改代码记录会发现提示 nothing to commit，working tree clean。说明刚才修改的代码都缓存起来了
17. 查看缓存的片段
```
git stash list
```
发现有缓存列表，刚才缓存的记录为 stash@{0}: XXXXXXXXXX
18. 还原缓存的代码
```
git stash apply stash@{0}
```
19. 查看提交的日志记录，即所有的历史记录
```
git log
```
20. 查看某次提交的内容
```
git show commit-id
```
21. 退回代码 回退到当前版本用HEAD表示当前版本，上一个版本是HEAD^,或者使用HEAD~1，表示上一个版本。HEAD后面是数字可以一直往大了写，只要有那么多老版本，HEAD后面也可以写版本编号，回到对应编号的版本
```
git reset --hard
```
