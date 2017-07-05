# http.basicauth
basicauth实现了HTTP的基本身份验证。 基本身份验证可使用用户名和密码来保护目录和文件。 注意，基本身份验证在普通的HTTP上是不安全的，当决定要使用HTTP基本身份验证来保护内容时，请谨慎使用。

当用户请求受保护的资源时，如果没有提供用户名和密码，浏览器就会提示用户输入用户名和密码。如果授权头部中存在正确的凭据，则服务器将授予对资源的访问权限，并将{user}占位符设置为用户名的值。 如果头信息丢失或凭据不正确，服务器将响应HTTP 401 Unauthorized。

这个指令允许使用htpasswd=的密码参数前缀和.htpasswd文件的路径来使用.htpasswd文件。 对.htpasswd的支持仅用于旧站点，将来可能会被删除，不要在新的站点上使用.htpasswd。


## 语法
```
basicauth path username password
```

*  **path** 要保护的文件或目录  
*  **username** 用户名  
*  **password** 密码  

这个语法对于使用默认的范围标志 “Restricted（受限）”来保护单个文件或基本路径/目录是非常方便的，如果要保护多个资源或指定范围，可以使用以下的写法：

```
basicauth username password {
    realm name
    resources
}
```

*  **username** 用户名   
*  **password** 密码  
*  **realm** 是指要保护的范围标志，它是可选的，不能重复。 Realms用于指定保护应用的空间，对于大多数浏览器来说，它可以很方便用户代理配置记住身份验证的详细信息。 
*  **resources** 要保护的文件/目录的列表，每行一个。


## 例子
保护在/secret文件夹中的所有文件，只有Bob可以通过密码"hiccup"访问它们：

```
basicauth /secret Bob hiccup
```

在"Mary Lou's documents"的范围保护多个文件和目录，所以Mary Lou可以通过她的密码"milkshakes"进行访问：

```
basicauth "Mary Lou" milkshakes {
	realm "Mary Lou's documents"
    /notes-for-mary-lou.txt
    /marylou-files
    /another-file.txt
}
```