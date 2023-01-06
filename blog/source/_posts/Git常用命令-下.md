---
title: Git常用命令(下)
date: 2023-01-05 23:06:49
categories:
- works
- SCU
- Git
tags:
- Git
---

## Git 常用命令(下)

- 本节主要介绍rebase相关的操作

- 我们将以具体的功能来处理你相关的事情

### 1. `Rebase`简介

![branch Basic][branch Basic]

- 相信你对于两个分支的`merge`操作并不陌生, 整合分支最容易的方法是` merge `命令。 它会把两个分支的最新快照`(C3 和 C4)`以及二者最近的共同祖先`(C2)`进行三方合并，合并的结果是生成一个新的快照（并提交）。

![Merge Branch][merge_branch]

- 你可以提取在 `C4` 中引入的补丁和修改，然后在 `C3` 的基础上应用一次。 在 `Git` 中，这种操作就叫做 **变基`（rebase）`**。 你可以使用 `rebase` 命令将提交到某一分支上的所有修改都移至另一分支上，就好像"重新播放"一样。

  - 在上述的场景中我们做如下操作便可以成功将`experiment`分支上的修改`C4`，应用到`master`分支上

  ```git
  git checkout experiment
  git rebase master
  ```

  - 它的原理是首先找到这两个分支（即当前分支 `experiment`、变基操作的目标基底分支 `master`） 的最近共同祖先 `C2`，然后对比当前分支相对于该祖先的历次提交，<font color=red>提取相应的修改并存为临时文件</font>， 然后将当前分支指向目标基底 `C3`, 最后以此将之前另存为临时文件的修改依序应用。

  - 它的过程就像下面描述的那样：

    ![Rebase Case][basic rebase 3]



### 2. 使用`rebase`合并代码 - 简单情况

- 切换到你要提取的分支

  ```git
  git switch "branchName"
  ```

- 使用`git rebase`将当前分支在与目标分支公共父节点之后的提交提取并应用到目标分支

  - 当前分支 - `experiment`
  - 目标分支 - `master`

  ```git
  git rebase master
  ```

- 如上图所示，会进行如图上的操作

  - 操作完成后`experiment`指向 - `C4'`节点

- 合并`experiment`的代码到`master`分支

  ```git
  git switch master
  git merge experiment
  ```

- 本此合并完成，同时你可以使用`rebase`合并之前的提交





[branch Basic]: https://raw.githubusercontent.com/Ranbun/images/main/blog/git/branch_basic.png "branch Basic"

[merge_branch]: https://raw.githubusercontent.com/Ranbun/images/main/blog/git/merge_branch_1.png "Merge branch"
[basic rebase 3]:https://raw.githubusercontent.com/Ranbun/images/main/blog/git/basic-rebase-3.png "basic rebase 3"

