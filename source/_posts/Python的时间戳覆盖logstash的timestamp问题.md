---
title: Python的时间戳覆盖Logstash的timestamp问题
date: 2018-12-11 18:45:50
tags: python
---
在将Python的日志导入到Elasticsearch的过程中，要用Logstash进行一次过滤和转换。
现在的遇到的一个问题是，timestamp的filter没有效果。原因是Python的时间戳默认使用微秒，而Logstash使用的是毫秒，两个时间的格式是无法匹配的。
