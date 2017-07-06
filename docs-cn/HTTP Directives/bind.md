# http.bind
bind overrides the host to which the server should bind. Normally, the listener binds to the wildcard host. However, you may force the listener to bind to another hostname or IP instead. This directive accepts only a host, not a port.
绑定将覆盖服务器应绑定到的主机，正常情况下，侦听器绑定到通配符主机。 但是，您可能会强制监听器绑定到另一个主机名或IP。 这个指令仅接受的是主机名而不是端口。

Note that binding sites inconsistently may result in unintended consequences. For example, if two sites on the same port resolve to 127.0.0.1 and only one of those sites is configured with bind 127.0.0.1, then only one site will be accessible since the other will bind to the port without a specific host; the OS will choose the more specific matching socket. (Virtual hosts are not shared across different listeners.)
注意，绑定站点不一致可能会导致意想不到的后果。 例如，如果同一端口上的两个站点解析为127.0.0.1，并且只有一个站点配置了bind 127.0.0.1，则只有一个站点将可访问，因为其他站点将绑定到没有特定主机的端口; 操作系统将选择更具体的匹配插座。 （虚拟主机不会在不同的监听器之间共享。）

## 语法
```
bind host
```

*  **host** 要绑定到的主机名（或IP地址）

## 例子
To make your socket accessible only to that machine, bind to IP 127.0.0.1 (localhost):  
要使你的socket只能访问该机器，请绑定到IP 127.0.0.1（localhost）：

```
bind 127.0.0.1
```