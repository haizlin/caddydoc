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

*  **extensions...** 以空格分隔的文件扩展用于压缩，支持通配符`*`以匹配所有扩展。
*  **paths** 以空格分隔不压缩的路径列表。 
*  **compression_level** 是从1（最快速度）到9（最佳压缩）的数字, 默认是9。
*  **min_bytes** 是在压缩发生之前响应需要的最小字节，默认值不是最小长度。

## 例子
开启gzip压缩：

```
gzip
```

启用了快速但最小的压缩，除了在/images和/videos文件夹（注意，无论如何图像和视频都不会gzip压缩）

```
gzip {
    level 1
    not   /images /videos
}
```