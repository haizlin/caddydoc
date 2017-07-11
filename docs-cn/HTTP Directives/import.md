# import
import 允许你使用其他文件中的配置，它被替换为这个文件的内容。

This is a unique directive in that import can appear outside of a server block. In other words, it can appear at the top of a Caddyfile where an address would normally be. Like other directives, however, it cannot be used inside of other directives.

这是一个特别的指令，这个导入可以出现在服务块之外，换句话说，它可以出现在一个地址通常是在一个Caddyfile的顶部，但是，像其他指令一样，它不能在其他指令中使用。

请注意，导入路径是相对于Caddyfile文件，而不是当前的工作目录。

## 语法
```
import pattern
```

pattern is the file or glob pattern (`*`) to include. Its contents will replace this line, as if that file's contents appeared here to begin with. This value is relative to the file's location. It is an error if a specific file cannot be found, but an empty glob pattern is not an error.  

*  **pattern** 是要包含的文件或glob模式(`*`), 它的内容将替代这一行，就好像该文件的内容出现在这里开始。 该值是相对于文件的位置。 如果找不到特定的文件，则是一个错误，但是一个空的glob模式不是错误。

## 例子  
导入公共的配置：

```
import config/common.conf
```

从vhosts文件夹中导入所有文件：

```
import ../vhosts/*
```