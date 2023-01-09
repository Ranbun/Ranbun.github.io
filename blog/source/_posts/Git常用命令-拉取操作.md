---
title: Git常用命令-拉取操作
date: 2023-01-09 13:47:18
categories:
- works
- SCU
- Git
tags:
- Git
---

## `Git`拉取操作

- 第一步，我们应该先克隆一个存储库，拥有一个存储库之后我们才可以进行后面的操作
- `git clone` - 克隆远程库
- `cd ...`

### 1. 查看所有分支

- 本地与远程的所有分支

```bash
git branch -a # 查看本地与远程的所有分支
git branch -r # 查看远程的所有分支

# 上述的两个分支都可以完成查看所有分支的功能
```

- 获取所有的远程分支

```shell
git fetch --all # 将远程仓库同步到本地,但并不会创建任何可以编辑的分支
```



### 2. 创建与远程分支关联的本地分支

- 你将要在本地获得一个分支,这个分支和远程的某个分支关联

- 从第一步获取的分支列表中选择一个分支

#### 2.1 使用`Git`默认操作

- 创建与远程分支同名的分支

```shell
# 系统自动与远程分支关联
git checkout targetBranchName
```

#### 2.2 创建分支指定远程分支

- 通过`git checkout`命令参数完成

```shell
git checkout -b localBranchName origin/targetBranchName 
# 使用此命令之后,本地分支localBranchName将与远程分支targetBranchName关联
# localBranchName 可以与targetBranchName同名也可以不同命,为了方便管理,强烈建议使用相同的名称
```

#### 2.3 指定跟踪的分支

- `track 远程分支`

```shell
git checkout --track origin/targetBranchName
# 创建与远程分支关联的分支
```

#### 2.4 拉取远程分支到本地

- 创建与远程分支关联的本地分支

```bash
git checkout -t origin/targetBranchName
```

#### 2.5 通过`Git Fetch`

```git
git fetch [<options>] [<repository> [<refspec>…​]]
git fetch [<options>] <group>
git fetch --multiple [<options>] [(<repository> | <group>)…​]
git fetch --all [<options>]
```

- `options`部分建议查阅官方文档
- `repository`
  - 远程仓库的名字
  - 也可以是 `origin`

- 获取远程分支到本地	

```bash
git fetch [repo] [targetBranchName]:[localBranchName]   
git fetch origin + origin_test:origin_test #将远程分支的内容下载到本地并在本地新建分支为origin_test
git fetch origin origin_test
# 此命令执行完后并不会切换到创建的分支,需要手动切换 
```



### 3. 编辑代码

- `do  something`

- ......

### 4. 推送代码到远程

- 一般情况是先做一次拉取 - `git pull or git fetch ......`
- 然后在进行推送 - `git push`

#### 4.1 `Git Pull`

- `git pull`并不是一个操作,一般是先执行`git fetch`然后执行一次`merge or rebase`操作









[Git]: https://git-scm.com/docs/git-fetch	"Git fetch"

