---
title: Git常用命令(上)
date: 2022-07-24 18:25:51
categories:
- works
- SCU
- Git
tags:
- Git
---

<p>
&ensp;&ensp;Git常用命令...
</p>

<!-- more -->

# Git常用命令


### `install Git`

- Git -- [官方网站](https://git-scm.com/)


### 常用命令

- 安装完成Git之后的操作 - 指定UserName & Email

    ```git
    git config --global user.name  "Your Git UserName"
    git config --global user.email "Your Email Address"
    ```

    -  一般时候在你安装完成之后需要做一下这个的配置。

- 生成密钥

    ```git
    ssh-keygen.exe -t rsa
    ```

    - 命令提示 
        ![Image](https://raw.githubusercontent.com/Ranbun/images/main/blog/git/ssh-genkey.png)

    - 然后将你的公钥添加为你所使用的远程管理平台对应SSH Key

- 创建本地版本库

    ```git
    mkdir repositoryDir  // 创建本地文件夹
    cd    repositoryDir  // 进入到文件夹
    git   init           // 初始化为本地仓库 创建.git目录
    ```

- 添加文件到本地暂存区&提交文件到本地

    ```git
    git add "fileName"       // 提交具体的文件 
    git add .                // 提交所有修改文件
    git commit -m  "备注信息" // 提交暂存区文件到本地
    ```

    - `git commit` 命令详解
        ![git commit](https://raw.githubusercontent.com/Ranbun/images/main/blog/git/git-commit.png)


- 版本控制

    ```git
    git log                      //查看提交历史记录，从最近到最远，可以看到3次
    git log --pretty=oneline     //加参数，简洁查看
    git reflog                   //查看每一次修改历史
    cat "fileName"                //查看文件内容 -- linux 命令
    git status                   //查看工作区中文件当前状态
    git reset --hard HEAD^(HEAD~100)(commit_id)  //回退版本
    git checkout -- "fileName"         //丢弃工作区的修改，即撤销修改
    git reset HEAD "fileName"          //丢弃暂存区的修改（若已提交，则回退）
    ```

- 删除文件

    ```git
    rm "filename"   // 删除单个文件 
    rm -rf *.*      // 强制递归删除所有文件
    git rm "filename"  // 直接删除
    // 恢复删除的文件
    git checkout -- "filename"
    ```

- 推送到远程的相关命令

    ```git
    ssh-keygen -t rsa -C "前面设置的Email"     //创建SSH Key 也可以不指定-C与邮件地址
    git remote add origin "git的远程仓库地址"  //关联远程仓库-源
    git push -u origin main                  //将本地内容推送到远程-第一次强制推送
    git push origin main                     //将本地内容推送到远程
    git remote -v                            //查看远程源信息
    git remote rm origin                     //删除远程源
    git clone "远程仓库地址可以是https的也可以是SSH(此时需要配置密钥)"  //克隆远程仓库
    git remote                               //查看远程库的信息
    git remote -v                            //查看远程库的详细信
    ```

- 分支管理

    ```git
    // 老版本的 git命令 
    git checkout -b main        //创建并切换到分支dev
    == 如下操作
    git branch   feature        //创建分支main
    git checkout feature        //切换到分支feature
    git checkout branchName origin/main    //拉取远程分支并切换,此时branchName分支对应得远程分支是main
    // 版本比较新一定的分支管理命令 
    git switch -c feature       //创建分支feature并切换
    git switch main             //直接切换到分支main
    // -----------------------------------------------------------------------
    git branch                  //查看当前分支
    git merge  feature          // 合并分支feature到当前分支(记得合并前拉取一下)
    git branch -d dev           // 删除dev分支 -d 强制删除
    ```

- 推送到远程

    ```git
    git push origin main                               //推送分支到远程
    git checkout -b feature origin/feature                     //创建远程origin的dev分支到本地
    git pull                                           //拉取远程分支内容
    git branch --set-upstream-to=origin/main main        //指定本地与远程dev的链接
    // 另一种合并操作，相比merge不会产生一次额外的提交
    git rebase                                         //把本地未push的分叉提交历史整理成直线
    ```



