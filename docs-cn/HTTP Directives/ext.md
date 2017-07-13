# http.ext
ext 允许你的网站有简洁的URL，当URL的路径没有一个扩展名结束时，它就会假设文件的扩展名。

在URL链接中的扩展是通过在路径中的最后一个点(`.`)元素来检测的

## 语法
```
ext extensions...
```

*  **extensions...** 是以空格分隔扩展名（包括点）的列表，扩展将按照列出的顺序进行尝试，它至少需要一个扩展名。

## 例子
假如你有一个文件是/contact.html，你可以通过设置扩展名.html，使用/contact访问该文件。

按以下顺序尝试 .html, .htm, .php

```
ext .html .htm .php
```