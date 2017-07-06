# http.gzip
如果客户端支持，gzip会启用gzip压缩。 默认情况下，响应是不会被gzip压缩的，如果启用，默认设置将确保图像，视频和存档（已经压缩）没有被压缩。

请注意，即使没有gzip指令，如果它们已经存在于磁盘上，并且客户端支持该编码，则Caddy将会为.gz（gzip）或.br（brotli）压缩文件提供服务。

## 语法
```
gzip
```

普通的gzip配置对于大多数情况都是足够好的，但如果需要，你可以获得更多的控制：

```
gzip {
    ext        extensions...
    not        paths
    level      compression_level
    min_length min_bytes
}
```

extensions... is a space-separated list of file extensions to compress. Supports wildcard *	to match all extensions.
*  **extensions...** 用于压缩以空格分隔的的文件扩展名的列表,支持通配符`*`以匹配所有扩展。
paths is a space-separated list of paths in which not to compress.
*  **paths** 是空格分隔的路径列表，其中不进行压缩。
compression_level is a number from 1 (best speed) to 9 (best compression). Default is 9.
min_bytes is the minimum number of bytes in a response needed before compression will happen. Default is no minimum length.

## 例子
开启gzip压缩：

```
gzip
```

Enable very fast but minimal compression except in the /images and /videos folders (note, however, that images and videos will not be gzipped anyway):
启用非常快速但最小的压缩，除了在/images和/videos文件夹（注意，但图像和视频不会gzip压缩）

```
gzip {
    level 1
    not   /images /videos
}
```