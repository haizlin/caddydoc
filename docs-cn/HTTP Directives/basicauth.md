# http.basicauth
basicauth implements HTTP Basic Authentication. Basic Authentication can be used to protect directories and files with a username and password. Note that basic auth is not secure over plain HTTP. Use discretion when deciding what to protect with HTTP Basic Authentication.
basicauth实现HTTP基本身份验证。 基本身份验证可用于使用用户名和密码来保护目录和文件。 请注意，基本身份验证在纯HTTP上不安全。 使用HTTP基本身份验证确定要保护的内容时，请谨慎使用。

When a user requests a resource that is protected, the browser will prompt the user for a username and password if they have not already supplied one. If the proper credentials are present in the Authorization header, the server will grant access to the resource and set the {user} placeholder to the value of the username. If the header is missing or the credentials are incorrect, the server will respond with HTTP 401 Unauthorized.
当用户请求受保护的资源时，如果用户名和密码尚未提供，则浏览器将提示用户输入用户名和密码。 如果授权标头中存在正确的凭据，则服务器将授予对资源的访问权限，并将{user}占位符设置为用户名的值。 如果标题丢失或凭据不正确，服务器将使用HTTP 401未经授权进行响应。

This directive allows use of .htpasswd files by prefixing the password argument with  htpasswd= and the path to the .htpasswd file to use. Support for .htpasswd is for legacy sites only and may be removed in the future; do not use .htpasswd with new sites.
该指令允许使用htpasswd =的password参数前缀和.htpasswd文件的路径来使用.htpasswd文件。 对.htpasswd的支持仅用于旧站点，可能会在将来被删除; 不要在新站点使用.htpasswd。


## 语法
```
basicauth path username password
```

[原]path is the file or directory to protect  
* `path`是要保护的文件或目录  
[原]username is the username  
* `username`是用户名  
[原]password is the password  
* `password`是密码  
This syntax is convenient for protecting a single file or base path/directory with the default realm "Restricted". To protect multiple resources or to specify a realm, use the following variation:  
该语法对于使用默认领域“受限制”保护单个文件或基本路径/目录非常方便。 要保护多个资源或指定领域，请使用以下变体：

```
basicauth username password {
    realm name
    resources
}
```


[原]username is the username.
* `username`是用户名  
[原]password is the password.
* `password`是密码  
realm identifies the protection partition; it is optional and cannot be repeated. Realms are used to specify the space in which the protection applies. This can be convenient for user agents that are configured to remember authentication details (which is most browsers).
* `realm`领域识别保护分区; 它是可选的，不能重复。 领域用于指定保护应用的空间。 这对于配置为记住身份验证详细信息（大多数浏览器）的用户代理可以方便。
resources is a list of files/directories to protect, one per line.
* `resources`是要保护的文件/目录的列表，每行一个。


## 例子
Protect all files in /secret so only Bob can access them with the password "hiccup":
保护在/secret文件夹中的所有文件，只有Bob可以通过密码"hiccup"访问它们：

```
basicauth /secret Bob hiccup
```

Protect multiple files and directories in the realm "Mary Lou's documents" so Mary Lou has access with her password "milkshakes":
保护多个文件和目录在“Mary Lou的文件”中，所以Mary Lou可以通过密码“milkshakes”访问：

```
basicauth "Mary Lou" milkshakes {
	realm "Mary Lou's documents"
    /notes-for-mary-lou.txt
    /marylou-files
    /another-file.txt
}
```