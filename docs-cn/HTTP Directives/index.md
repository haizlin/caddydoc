# http.index

index指令是用来设置"索引"文件的文件名列表，当请求的是目录路径而不是指定文件时，将检查目录中现有的索引文件，使用第一个匹配的文件名。

以下是默认的索引文件顺序：

1. index.html
2. index.htm
3. index.txt
4. default.html
5. default.htm
6. default.txt
 
如果使用这个指令将覆盖它的默认列表，而不是追加。

## 语法
```
index filenames...
```
*  **filenames...** 是以空格分隔的文件名列表用来设置索引, 它至少需要一个文件名。

## 例子

只使用goaway.png和easteregg.html作为索引文件：

```
index goaway.png easteregg.html
```