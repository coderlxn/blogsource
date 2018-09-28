---
title: Python问题收集
date: 2018-09-26 09:39:49
tags: Python
---
#### python引入模块时import与from ... import的区别
* import datetime是引入整个datetime包，如果使用datetime包中的datetime类,需要加上模块名的限定。
```python
import datetime
print datetime.datetime.now()
```
* from datetime import datetime是只引入datetime包里的datetime类,在使用时无需添加模块名的限定。
```python
from datetime import datetime
print datetime.now()
```