---
layout: post
title: Git的各种撤销操作
category: git
keywords: "git,undo,撤销"
date: 2015-06-14 12:32:48
last_modified_at: 2015-06-14 12:32:48
---
使用版本控制工具`Git`最大的好处就是，发生错误操作、修改后可以恢复到原来最后一次正确的状态

### 撤销git push操作

运行`git push`后，会将新的`commit`提交到远程仓库。可以使用`git revert`来撤销操作

```
git revert <SHA>
```

`git revert`会创建一个新的`commit`，其中的修改与`<SHA>`的`commit`相反，以此来达到撤销之前操作的目的。

### 修改commit信息

当使用`git commit -m "Fxies bug #42"`提交了一个`commit`后发现信息写错了，可以使用`git commit --amend -m "Fixes bug #42"`来修改。这条命令会将最近提交的`commit`信息修改

### 撤销当前编辑的内容

当编辑了代码并保存后，发现写错了，但是没有`commit`，想要把文件恢复成原来的样子，使用

```
git checkout -- <bad filename>
```

`<bad filename>`可以是分支名或指定的`SHA`，默认下会回到最近的一次`commit`
	
### 撤销commit

在提交了几次`commit`后，觉得都不好，想要把这几次`commit`全都撤销掉

```shell
git reset <last good SHA> 
git reset --hard <last good SHA>
```

`<last good SHA>`指想要恢复到的`commit`。`git reset`将`commit`撤消后代码的变动还在，但没有被`commit`；加上`--hard`会将代码的变动一并撤销
	
### 恢复撤销的commit

在使用`git reset --hard`撤销了`commit`后，又想要恢复，可以使用`git reflog` 和 `git reset`或`git checkout`

- `git reflog`只能当`HEAD`发生改变后使用，运行`git reflog`后可以看到能够恢复的`commit`
- 如果是想要恢复`git`工程的历史，使用`git reset --hard <SHA>`
- 如果是想要重新创建删除的文件，并且不修改历史记录，使用`git checkout <SHA> -- <filename>`
- 如果是想重做某个`commit`使用`git cherry-pick <SHA>`
	
### 撤销提交到master分支的修改

在提交了几个`commit`后发现是`master`分支，想要把这些`commit`提交到其他分支上去，可以使用`git branch feature`, `git reset --hard origin/master`, 和 `git checkout feature`<br/>
`git branch feature`创建一个新的分支叫`feature`，`git reset --hard origin/master`撤销对`master`分支的修改，`git checkout feature`将当前分支切换到`feature`

### 撤销已记录的文件

不小心将`application.log`添加到了仓库中(tracked file)，撤销这个操作可以使用`git rm --cached application.log`

## 参考链接

- [How to undo (almost) anything with Git](https://github.com/blog/2019-how-to-undo-almost-anything-with-git)
- [Git - Reference](http://git-scm.com/docs)