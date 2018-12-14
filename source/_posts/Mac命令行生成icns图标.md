---
title: Mac命令行生成icns图标
date: 2018-12-11 14:14:33
tags: mac
---
1. 创建文件夹'xxx.iconset'
1. 在xxx.iconset中创建 16X16 到 1024 * 1024尺寸的icon,需要命名为xxx_16x16.png，xxx_16x16@2x.png
1. 通过明执命令，即可生成icns文件
```bash
iconutil -c icns yum.iconset -o yum.icns
```