# http.ext
ext allows your site to have clean URLs by assuming a file extension when the path of the URL does not end with one.
ext允许您的网站在URL的路径不以一个结尾的情况下假定文件扩展名来具有干净的URL。

An extension in the URL is detected by checking the last element of the path for a dot (.).
通过检查点（.）的路径的最后一个元素来检测URL中的扩展。
在链接中的扩展是通过检测在路径中的最后一个点(.)元素

## 语法
```
ext extensions...
```

*  **extensions...** is a list of space-separated extensions (including the dot) to try. Extensions will be tried in the order listed. At least one extension is required.
*  **extensions...** 是要尝试的空格分隔扩展名（包括点）的列表，扩展程序将按照列出的顺序进行尝试，至少需要一个扩展。

## 例子
Suppose you have a file called /contact.html. You could serve that file at /contact by having Caddy try .html files.
假如你有一个文件/contact.html，你可以通过让Caddy尝试.html文件来提供/contact该文件。


按以下顺序尝试 .html, .htm, .php

```
ext .html .htm .php
```