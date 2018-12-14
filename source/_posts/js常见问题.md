---
title: js常见问题
date: 2018-11-13 16:36:55
tags: javascript
---

#### 判断对象是否定义
在访问一个不确定是否存在的对象前，需要判断对象是否已定义。
如果直接使用
```js
if (someobject) {
	// code
}
```
这中形式，会提示undefined错误，可以使用 `typeof` 关键词来获取类型进行对比
```js
if (typeof someobject !== "undefined") {
	// code
}
```

