---
title: git使用
date: 2017-09-02 11:20:48
tags:
---

### git command

``` bash
git clone address: 克隆一个仓库到本地
git branch: 查看分支
git branch childBranch: 新建分支
git checkout childBranch: 切换分支
git add .： 把我们要提交的文件信息添加到索引库中
git commit -m message: 根据索引库中的内容来进行文件的提交
git push --set-upstream origin childBranch: 设置当前的分支跟踪远程的childBranch分支，以后可使用git push提交分支到childBranch。
git push：提交分支到master（默认）.
git merge branchName: 将branchName分支合并到当前分支
```
