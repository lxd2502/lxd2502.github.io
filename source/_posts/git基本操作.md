---
title: git基本操作
date: 2018-09-05 16:54:28
categories: 工具
tags: git
---

### git基本操作
#### fetch操作：
```
git fetch
```

#### git拉取远程分支：
```
git checkout -b 本地分支名 origin/远程分支名
```

#### git创建本地分支：
```
git checkout -b 新分支名
```

#### git查看本地分支：
```
git branch
```

#### git切换分支：
```
git checkout branchName
```

#### git重命名本地分支：
```
git branch -m oldName newName
```

#### git关联远程分支：
```
查看关联的远程分支：
git branch -vv

关联一个远程分支：
git branch --set-upstream-to=远程分支 本地分支

注：本地分支省略，默认关联的是当前本地分支
```
#### git本地分支推送到远程分支：
```
a,没有同名的远程分支，创建一个新的远程分支
git push origin branchName
git push origin branchName:branchName

b,推送到指定的远程分支
git push origin branchName:targetName
```
#### git删除本地、远程分支：
```
a，删除本地分支
git branch -d branchName
git branch -D branchName(有未合并时强制删除)

b，删除远程分支
git push origin --delete branchName
```

#### git rebase操作：
```
git rebase branchName (表示rebase一个branchName分支到当前分支)

rebase有冲突的时候，使用“git status”查看哪个文件有冲突，打开对应文件解决冲突后，继续rebase：
git add -u
git rebase --continue

注⚠️：
<<<<<<< 表示冲突代码开始位置
======= 表示冲突代码分割位置
>>>>>>> 表示冲突代码结束位置

如果rebase过程中，想中途退出，恢复之前的代码可使用--abort
git rebase --abort


流程：
git rebase xxx
while( 有冲突 ) {
    git status
    找到冲突所在文件，解决冲突
    git add -u
    git rebase --continue
    if (git branch --abort)
        break;
}
```

#### git merge操作：
```
git fetch
git merge aaa
```

#### git reset操作：
```
回退一个版本：
git reset HEAD^

回退n个版本：
git reset HEAD~n

注⚠️：回退参数：
soft： git reset --soft HEAD~1，软回退一个版本，所谓软回退是指本地版本库的头指针全部重置到指定版本，且将本次提交之后的所有变更都移动到暂存区。

mixed：git reset HEAD~1，回退一个版本，就是指本地版本库的头指针全部重置到指定版本，且会重置暂存区，即这次提交之后的修改都移动到未暂存阶段。

hard：git reset --hard HEAD~1，硬回退一个版本，就是指将本地版本库的头指针全部重置到指定版本，且不仅会重置暂存区，还会将工作区代码也退回到这个版本。
```

#### git追加提交：
```
追加提交的情况：
a，对未merge的提交不满意，希望修改其中某些信息，如代码，提交描述等，可以使用"git commit —amend”进行追加提交，有优点是不会产生新的commit id。
```

#### git丢弃本地修改：
```
git checkout . (只放弃本地修改，新增、删除并没有恢复过来)

git checkout . && git clean -df (可以放弃所有的修改、添加、删除)

注⚠️：git clean -df中，-d表示同时移除目录，-f表示force。因为在git配置文件中，clean.requireForce=true, 如果不加-f，clean将会拒绝进行。
```
#### git暂存
```
git stash可用来暂存当前正在进行的工作，返回到上一个commit。比如为了修复一个bug，先stash，使自己返回到上一个commit，改完bug之后再stash pop，继续原来的工作：

流程：
git stash save “xxx” （暂存，暂存信息为xxx）
// do sth
git stash list                 (list stashed changes in this git )
git show stash@{0}             (see the last stash )
git stash pop                  (apply last stash and remove it from the list)
git stash apply stash@{1}      (apply first stash)
git stash clear                (clear stash)
git stash --help
```

#### git黑魔法：
```
今天使用sourceTree提交代码的时候，commit之后未submit，直接rebase主分支代码，完了发现自己本地做的修改都没了，且远程没有本地分支。google之后发现有一个简单方法可以恢复到本地commit版本. 类似情况可用如下操作：
git reflog show develop (查找develop分支所有操作日志)
git reset --hard develop@{4}
```

#### 删除本地有，远程库已不存在的分支
```
git remote prune origin
```













