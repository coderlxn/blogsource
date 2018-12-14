---
title: git使用depth拉取后获取完整分支
date: 2018-12-07 10:21:56
tags: git
---
在拉去git仓库时，如果使用了depth=1之类的参数，拉去的数据只有master分支的最新节点。
可以使用fetch来获取更多的节点数据。
```
git fetch  --unshallow
```
但仍然只有master分支的数据，这个时候执行git checkout会提示其他的分支不存在，可以安装下面的步骤来获取完整的分支数据。
```
1. git remote set-branches origin '*'
2. git fetch -v
3. git checkout the-branch-i-ve-been-looking-for
```