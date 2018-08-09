# Blog

## 命令

生成博客

```sh
hugo --cleanDestinationDir
```

## 一些操作记录

添加子目录，建立与git项目的关联
建立关联总共有2条命令。

语法：git remote add -f <子仓库名> <子仓库地址>

解释：其中-f意思是在添加远程仓库之后，立即执行fetch。

语法：git subtree add –prefix=<子目录名> <子仓库名> <分支> –squash

解释：–squash意思是把subtree的改动合并成一次commit，这样就不用拉取子项目完整的历史记录。–prefix之后的=等号也可以用空格。

示例

```sh
$git remote add -f theme https://github.com/aoxu/theme.git
$git subtree add --prefix=theme theme master --squash
```

从子目录push到远程仓库（确认你有写权限）
推送子目录的变更有1条命令。

语法：git subtree push –prefix=<子目录名> <远程分支名> 分支

示例

$git subtree push --prefix=theme theme master

### tip

腾讯万象优图地址

http://linhsblog-10013469.image.myqcloud.com

推送主题到git

git subtree push --prefix=themes/tranquilpeak theme master