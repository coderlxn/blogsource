---
title: 通过Bash脚本SSH登录后执行命令
date: 2018-11-02 10:24:36
tags: shell
---
现在遇到这么一个场景：
有一些服务需要部署在服务器上，而且本地机器和目标服务器之间有一个跳板机器。因为有几个服务文件，每次部署的时候需要分别拷贝，非常麻烦，而且容易出错。
所以考虑通过Bash脚本来实现。经过一番搜索，发现实现的方式也很简单，基本的语法是
```bash
ssh server << EOF
	command
EOF
```
即使中间有跳板级，也只需要再嵌套一层即可
```bash
# 获取git的提交次数
REV=`git rev-list HEAD | wc -l | awk '{print $1}'`
echo "build version : " $REV

ssh -i ~/Documents/aws/password.pem ubuntu@127.0.0.147 << EOF
	echo 'current version ' $REV
	ssh -i pems/password.pem ubuntu@127.0.0.157 << EOF
	ls
	echo 'hello version ' $REV
EOF
```