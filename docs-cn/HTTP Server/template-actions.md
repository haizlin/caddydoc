# 模板操作
Caddy的模板功能可以给静态网站添加一些动态内容，并能帮助你减少重复的页面。 模板支持一些不同的指令，例如[templates](../HTTP Directives/templates)，[browse](../HTTP Directives/browse)和[markdown](../HTTP Directives/markdown)。

> 模板操作只适用于支持它们的指令, 检查指令的文档，看看它们是如何使用的。

Caddy模板使用了Go的[text/template](http://golang.org/pkg/text/template/)包中定义的语法，了解[text/template](http://golang.org/pkg/text/template/)将有助于你充分利用模板。但对于那些不是程序员的人来说，这里有一些简化的说明。 Caddy还为模板网页添加了一些特别有用的功能，我们在这里记录了这些功能。

## 基本语法
模板操作包含在`{{`和`}}`标记之间，模板关键词是区分大小写的。

## 公共函数
**引入其它的文件:**  
```
{{.Include "path/to/file.html"}}  // 没有参数
{{.Include "path/to/file.html" "arg1" 2 "value 3"}}  // 有参数
```

**在引入的文件中获取参数：**  
```
{{index .Args 0}} // 0是第一个参数，依此类推
```

**引入并渲染一个Markdown文件：（不需要markdown中间件）**  
```
{{.Markdown "path/to/file.md"}}
```

**显示当前的时间戳:(格式)**  
```
{{.Now "Monday, 2 Jan 2006"}}
```

**cookie的值：**  
```
{{.Cookie "name"}}
```

**头字段的值：**  
```
{{.Header "name"}}
```

**访客的IP：**  
```
{{.IP}}
```

**服务器的IP：**  
```
{{.ServerIP}}
```

**访客的主机（需要查找DNS，可能很慢）:**  
```
{{.Hostname}}
```

**请求的URI:**  
```
{{.URI}}
```

**请求的主机：**  
```
{{.Host}}
```

**请求的端口：**  
```
{{.Port}}
```

**请求的方法：**  
```
{{.Method}}
```

**请求路径是否匹配其它的路径：**  
```
{{.PathMatches "/some/path"}}
```

**URL的一部分：**  
```
{{.URL.RawQuery}}
```

RawQuery 返回查询字符串，你可以使用Host，Scheme，Fragment，String或Query.Get "参数"来替换RawQuery。

**环境变量:**  
```
{{.Env.ENV_VAR_NAME}}
```

**分割字符串：（从开头或者结尾分割）** 
```
{{.Truncate "value" 3}}  // "val"
{{.Truncate "value" -3}} // "lue"
```


**字符串替换：**  
```
{{.Replace "haystack" "needle" "replacement"}}
```

**当前日期/时间对象：（在和日期相关的函数中有用）**  
```
{{.NowDate}}
```

**获取文件的拓展名：**
```
{{.Ext "path/filename.ext"}}
```

**从文件名中剥离出拓展名：**
```
{{.StripExt "filename.ext"}}
```

**剥离HTML标签留下纯文本：**  
```
{{.StripHTML "Shows <b>only</b> text content"}}
```

**小写字符串：**  
```
{{.ToLower "Makes Me ONLY lowercase"}}
```

**大写字符串：**  
```
{{.ToUpper "Makes me only UPPERCASE"}}
```

**用分隔符分隔字符串：**  
```
{{.Split "123-456-7890" "-"}}
```

**将值转换为切片（数组）：**  
```
{{.Slice "a" "b" "c"}}
```

**键值映射：（在高级情况下，使用子模板等）**  
```
{{.Map "key1" "value1" "key2" "value2"}}
```

**列出目录中的文件：**  
```
{{.Files "sub/directory"}}
{{.Context.Files "sub/directory"}} // templates 在markdown模板中的前缀.Context
```

**是否有可能拦截HTTPS：**  
```
{{.IsMITM}}
```

**最小和最大长度之间随机长度的字符串：**  
```
{{.RandomString 100 10000}}
```

## 内置清理功能
这些功能内置在text/template中，但是你会发现他们很有用。

**让HTML安全（转义特殊字符）：**
```
{{html "Makes it <i>safe</i> to render as HTML"}}
```

**让Javascript安全：**  
```
{{js "Makes content safe for use in JS"}}
```

**URL转义：（编码查询内容）**  
```
{{urlquery "Makes safe for URL query strings"}}
```

## 控制语句
**If:**  
```
{{if .PathMatches "/secret/sauce"}}
	Only for secret sauce pages
{{end}}
```

**If-else:**  
```
{{if .PathMatches "/secret/sauce"}}
	Only for secret sauce pages
{{else}}
	No secret sauce for you
{{end}}
```

**If-elseif-else:**  
```
{{if .PathMatches "/secret/sauce"}}
	Only for secret sauce pages
{{else if eq .URL "/banana.html"}}
	You're on the banana page
{{else}}
	No bananas or secret sauce
{{end}}
```

**Range: (遍历数据，此示例转储请求头)** 
```
{{range $field, $val := .Req.Header}}
    {{$field}}: {{$val}}
{{end}}
```

**服务器端注释：**  
```
{{/* This isn't sent to the client */}}
```

## 比较函数
在"if"语句中你可以使用比较的方法：

* eq 相等
* ne 不相等
* lt 小于
* le 小于或等于
* gt 大于
* ge 大于或等于

或者这些逻辑语句：
* not 反转if条件
* or 返回第一个非空参数或最后一个参数
* and 返回第一个空参数或最后一个参数

## 深入阅读  
这些只是你可以做的几个例子，如果你需要更多的模板功能，请阅读 [text/template](http://golang.org/pkg/text/template/) 包获得更详细的信息。
