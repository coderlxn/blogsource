---
title: Go开源框架
date: 2018-09-20 14:04:07
tags:
---
##### Beego
beego是一个用Go开发的应用框架，思路来自于tornado，路由设计来源于sinatra，支持如下特性:
1. MVC
2. REST
3. 智能路由
4. 日志调试
5. 配置管理
6. 模板自动渲染
7. layout设计
8. 中间件插入逻辑
9. 方便的JSON/XML服务

[官网](http://beego.me)
##### Gin
Gin is a HTTP web framework written in Go (Golang). It features a Martini-like API with much better performance -- up to 40 times faster. If you need smashing performance, get yourself some Gin.
Gin是一个用Go（Golang）编写的HTTP Web框架。 它具有类似Martini的API，具有更好的性能 - 速度提高了40倍。 如果你需要更高的性能，试试Gin。
[官网](https://gin-gonic.github.io/gin/) [源码](https://github.com/gin-gonic/gin)
##### Martini
Martini是一个强大为了编写模块化Web应用而生的GO语言框架.
* 使用极其简单.
* 无侵入式的设计.
* 很好的与其他的Go语言包协同使用.
* 超赞的路径匹配和路由.
* 模块化的设计 - 容易插入功能件，也容易将其拔出来.
* 已有很多的中间件可以直接使用.
* 框架内已拥有很好的开箱即用的功能支持.
* 完全兼容http.HandlerFunc接口.

[源码](https://github.com/go-martini/martini)
##### Buffalo
Buffalo is a Go web development eco-system. Designed to make the life of a Go web developer easier.

Buffalo starts by generating a web project for you that already has everything from front-end (JavaScript, SCSS, etc...) to back-end (database, routing, etc...) already hooked up and ready to run. From there it provides easy APIs to build your web application quickly in Go.

Buffalo isn't just a framework, it's a holistic web development environment and project structure that lets developers get straight to the business of, well, building their business.

Buffalo是Go网站开发生态系统。旨在使Go Web开发人员的生活更轻松。

Buffalo首先为您生成一个Web项目，该项目已经包含从前端（JavaScript，SCSS等）到后端（数据库，路由等）已经连接并准备运行的所有内容。 从那里，它提供了简单的API，可以在Go中快速构建您的Web应用程序。

Buffalo不仅仅是一个框架，它是一个整体的Web开发环境和项目结构，让开发人员可以直接开展业务，并建立自己的业务。

[源码](https://github.com/gobuffalo/buffalo)

##### Echo
High performance, minimalist Go web framework
高性能，极简主义的Go Web框架
* 优化的HTTP路由器，巧妙地优先考虑路由
* 构建健壮且可扩展的RESTful API
* API组
* 可扩展的中间件框架
* 在root，group或route级别定义中间件
* JSON，XML和表单有效内容的数据绑定
* 方便的功能，可以发送各种HTTP响应
* 集中式HTTP错误处理
* 模板渲染与任何模板引擎
* 定义记录器的格式
* 高度可定制
* 通过Let的加密自动TLS
* HTTP / 2支持

[源码](https://github.com/labstack/echo) [官网](https://echo.labstack.com/) 

##### Iris
The fastest backend community-driven web framework on (THIS) Earth. HTTP/2, MVC and more. Can your favourite web framework do that?
Iris is a fast, simple yet fully featured and very efficient web framework for Go.

Iris provides a beautifully expressive and easy to use foundation for your next website or API.

Iris offers a complete and decent solution and support for all gophers around the globe.

地球上最快的后端驱动的Web框架。 HTTP / 2，MVC等。 您最喜欢的Web框架可以做到吗？
Iris是一个快速，简单但功能齐全且非常高效的Go语言Web框架。

Iris为您的下一个网站或API提供了一个精美的表达和易于使用的基础。

Iris为全球所有gophers提供完整，体面的解决方案和支持。
[源码](https://github.com/kataras/iris) [官网](https://iris-go.com/) 
##### Revel
A high productivity, full-stack web framework for the Go language. 
一个基于Go语言的高生产力，全栈Web框架。
[源码](https://github.com/revel/revel)