---
title: 学习git
category: 学习
---


### git教程-廖雪峰



```bash
mkdir learngit
cd learngit
git init

# 然后可以创建a.txt
git add a.txt 
git commit -m "write new file a.txt" # 提交修改

# 修改a.txt的内容之后，可以查看哪些文件被修改了
git status
git diff

# 然后可以再次提交
git add a.txt
git status #查看将要提交修改的文件
git commit -m "add a new line"

git log
git log --pretty=oneline

git reset --hard head^
cat a.txt
git reflog
git reset --hard 9611
cat a.txt
```

工作区的概念

```bash
vim a.txt # 修改文件
echo catwudi > license # 创建一个新文件
git status
git add a.txt
git add license
git status
git commit -m "工作区是如何工作的"
git log
```

撤销修改，就是让这个文件回到最近一次`git commit`或`git add`时的状态。

```bash
git checkout -- a.txt

# 如果已经add到暂存区了
git reset head a.txt
git checkout -- a.txt
```

删除文件

```bash
# 误删
rm license
git checkout -- license

# 真删
git rm license
git commit -m "remove license"
```

远程仓库

```bash
ssh-keygen -t rsa -C "245593007@qq.com"
```

在`~/.ssh`中会生成两个文件，其中`id_rsa.pub`是公钥，配置到github的设置上，

```bash
git remote add origin git@github.com:dxlzlcy/learngit.git

git push -u origin master #第一次时
git push origin master
```

从远程库克隆

```bash
git clone git@github.com:dxlzlcy/dxlzlcy.github.io # ssh协议
git clone https://github.com/dxlzlcy/dxlzlcy.github.io.git # http协议
```

分支管理

```bash
git checkout -b dev
# git branch dev
# git checkout dev # 切换分支

git branch # 查看

echo hello > helloworld
git add helloworld
git commit -m "branch test"

git checkout master
ls

git merge dev
ls

git branch -d dev
```


