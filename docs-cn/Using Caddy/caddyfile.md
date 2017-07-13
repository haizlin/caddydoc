# Caddyfile 语法

This page describes the syntax of the Caddyfile. If it is your first time writing a Caddyfile, try the Caddyfile primer tutorial instead. This page is not beginner-friendly; it is technical and kind of boring.

Although this article is verbose, the Caddyfile is designed to be easily readable and writable by humans. You will find that it is easy to remember, not cumbersome, and flows off the fingers.

The term "Caddyfile" often refers to a file, but more generally means a blob of Caddy configuration text. A Caddyfile can be used to configure any Caddy server type: HTTP, DNS, etc. The basic structure and syntax of the Caddyfile is the same for all server types, but semantics change. Because of this variability, this document treats the Caddyfile only as the generic configuration syntax as it applies to all server types. Caddyfile documentation for specific types may be found within their respective docs. For instance, the HTTP server documents the semantics of its Caddyfile.

这个页面介绍了Caddyfile的语法，如果你是第一次写Caddyfile，请阅读 [Caddyfile](https://caddyserver.com/tutorial/caddyfile) 入门教程。 这个页面不是初学者; 它是技术性有点枯燥。
虽然这篇文章是冗长的，但是Caddyfile的设计易于人类读取和写入。 你会发现很容易记住，不麻烦，流出手指。

The term "Caddyfile" often refers to a file, but more generally means a blob of Caddy configuration text. A Caddyfile can be used to configure any Caddy server type: HTTP, DNS, etc. The basic structure and syntax of the Caddyfile is the same for all server types, but semantics change. Because of this variability, this document treats the Caddyfile only as the generic configuration syntax as it applies to all server types. Caddyfile documentation for specific types may be found within their respective docs. For instance, the HTTP server [documents the semantics of its Caddyfile](/docs/http-caddyfile).

“Caddyfile”这个词通常指的是一个文件，但更一般地意味着一个球形的配置文本。 Caddyfile可用于配置任何Caddy服务器类型：HTTP，DNS等.Carddyfile的基本结构和语法对于所有服务器类型都是相同的，但语义变化。 由于这种变化性，本文档仅将Caddyfile视为适用于所有服务器类型的通用配置语法。 可以在各自的文档中找到具体类型的Caddyfile文档。 例如，HTTP服务器[记录其Caddyfile的语义]（/ docs / http-caddyfile）

**主题**
1.  [文件和编码](#format)
2.  [词汇语法](#lexical-syntax)
3.  [结构](#structure)
4.  [标签](#labels)
5.  [指令](#directives)
6.  [环境变量](#env)
7.  [导入](#import)
8.  [例子](#examples)

## 文件和编码 <span id="format"></span>

The Caddyfile is plain Unicode text encoded with UTF-8. Each code point is distinct; specifically, lowercase and uppercase characters are different. A leading byte order mark (0xFEFF), if present, will be ignored.
Caddyfile是用UTF-8编码的纯Unicode文本，每个代码点是不同的; 确切地说，大小写字符是不同的。 前导字节顺序标记（0xFEFF）（如果存在）将被忽略。

## 词汇语法 <span id="lexical-syntax"></span>

A token is a sequence of whitespace-delimited characters in the Caddyfile. A token that starts with quotes " is read literally (including whitespace) until the next instance of quotes " that is not escaped. Quote literals may be escaped with a backslash like so: \". Only quotes are escapable. “Smart quotes” are not valid as quotes.
在Caddyfile中的标志是一系列以空格分隔的字符。 以引号开头的标志"字面上（包括空格），直到引用的下一个引号”未被转义。 报价字面值可以使用如下反斜杠进行转义：\“。只有引号可以转义。”智能引号“无效。

**Lines** are delimited with the `\n` (newline) character only. Carriage return `\r` is discarded unless quoted. Blank, unquoted lines are allowed and ignored.  
`行`仅用`\n`（换行符）字符分隔。 除非引用，否则回车`\r`将被丢弃。 空白的，无引号的行将被允许和忽略。

**Comments** are discarded by the lexer. Comments begin with an unquoted hash `#` and continue to the end of the line. Comments may start a line or appear in the middle of a line as part of an unquoted token. For the purposes of this document, commented and blank lines are no longer considered.
`注释`在记法分析时将被丢弃，注释以无引用的引号`#`开始直到本行尾结束。注释可能会开始一行或出现在行的中间作为不引用标记的一部分。为了本文档的目的，不再考虑注释和空白行。

Tokens are then evaluated by the parser for structure.
然后由解析器对结构进行令牌的评估。

## 结构 <span id="structure"></span>
A Caddyfile has no global scope. The most global unit of the Caddyfile is an entry. An entry consists of a list of labels and a definition associated with those labels. A label is a string identifier, and a definition is a body (one or more lines) of tokens grouped together in a block:
Caddyfile没有全局作用域，Caddyfile中更多的全局单元入口。 条目由标签列表和与这些标签相关联的定义组成。 标签是字符串标识符，定义是块中组合在一起的标记（一行或多行）的标记：

```
list of labels
definition (block)
```


A Caddyfile with only one entry may consist simply of the label line(s) followed by the definition on the next line(s), as shown above. However, a Caddyfile with more than one entry must enclose each definition in curly braces { }. The opening curly brace { must be at the end of the label line, and the closing curly brace } must be the only token on its line:
只有一个条目的卡迪文件可以仅包含标签行，后面是下一行的定义，如上所示。 但是，具有多个条目的Caddyfile必须将每个定义都包含在花括号{}中。 开放的大括号{必须在标签行的末尾，而关闭大括号}必须是其行上唯一的标记：

```
list of labels {
	definition (block)
}

list of labels {
	definition (block)
}
```

Consistent tab indentation is encouraged within blocks enclosed by curly braces.
鼓励使用大括号括在块内的一致的标签缩进。

**The first line of a Caddyfile is always a label line.** Comment lines, empty lines, and [import](/docs/import) lines are the exceptions.  
`Caddyfile的第一行始终是标签行。` 注释行，空行和[导入](https://caddyserver.com/docs/import)行都是例外。

## 标签 <span id="labels"></span>

Labels are the only tokens that appear outside of blocks (with one exception being the import directive). A label line may have just one label:  
标签是出现在块外的唯一标记（除了一个例外是导入指令）。 一个标签行可能只有一个标签：
```
label
```
or several labels, separated by spaces:  
或多个标签，以空格分隔：

```
label1 label2 ...
```
If many labels are to head a block, the labels may be suffixed with a comma. A comma-suffixed label may be followed by a newline, in which case the next line will be considered part of the same line:

```
label1,
label2
```
Mixing of these patterns is valid (but discouraged), as long as the last label of the line has a comma if the next line is to continue the list of labels:

```
label1 label2,
label3, label4,
label5
```
A definition with multiple labels is replicated across each label as if they had been defined separately but with the same definition.  
具有多个标签的定义在每个标签上进行复制，就像它们已经单独定义但具有相同定义一样。

## 指令 <span id="directives"></span>

The body of the definition follows label lines. The first token of each line in a definition body is a **directive**. Every token _after_ the directive on the same line is an **argument**. Arguments are optional:  
定义的正文遵循标签行。 定义体中每行的第一个标记是一个指令。 在同一行上的指令之后的每个令牌都是一个参数。 参数是可选的：

```
directive1
directive2 arg1 arg2
directive3 arg3
```

Commas are not acceptable delimiters for arguments; they will be treated as part of the argument value. Arguments are delimited solely by same-line whitespace.
逗号不是可接受的参数分隔符; 它们将被视为参数值的一部分, 参数仅由同一行空格分隔。

Directives may span multiple lines by opening a block. Blocks are enclosed by curly braces `{ }`. The opening curly brace `{` must be at the end of the directive's first line, and the closing curly brace `}` must be the only token on its line:

```
directive {
    ...
}
```

Within a directive block, the first token of each line may be considered a subdirective or property, depending on how it is used (other terms may be applied). And as before, they can have arguments:
在指令块内，每行的第一个令牌可能被视为子目录或属性，具体取决于它的使用方式（可以应用其他术语）。 和以前一样，他们可以有论据：

```
directive arg1 {
    subdir arg2 arg3
    ...
}
```

Subdirectives cannot open new blocks. In other words, nested directive blocks are not supported. If a directive block is empty, the curly braces should be omitted entirely.

## 环境变量 <span id="env"></span>

任何令牌（标签，指令，参数等）可能包含或仅由环境变量组成，该变量将Unix窗体或Windows窗体包含在大括号`{ }`中，而不需要额外的空格：

```
label_{$ENV_VAR_1}
directive {%ENV_VAR_2%}
```

Either form works on any OS. A single environment variable does not expand out into multiple tokens, arguments, or values.

## 导入 <span id="import"></span>

The [import](/docs/import) directive is a special case, because it can appear outside a definition block. The consequence of this is that no label can take on the value of "import".
这个[导入]（/docs/import）指令是一种特殊情况，因为它可以出现在定义块之外， 这样做的结果是没有标签可以承担“导入”的值。


Where an import line is, that line will be replaced with the contents of the imported file, unmodified. See the import docs for more information.
如果导入行是，该行将被替换为导入的文件的内容，未修改。 有关详细信息，请参阅[导入文档]（/ docs / import）。

## 例子 <span id="examples"></span>
A very simple Caddyfile with only one entry:  
一个非常简单的Caddyfile仅仅只有一个入口：  
```
label1

directive1 argument1
directive2
```

Appending the prior example with another entry introduces the need for curly braces:
在前面的例子中添加另一个条目介绍了花括号的必要性：
使用另一个条目附加先前的例子介绍了花括号的需要：
```
label1 {
	directive1 arg1
	directive2
}
label2, label3 {
	directive3 arg2
	directive4 arg3 arg4
}
```

Some people prefer to always use braces even if there's just one entry; this is fine, but unnecessary:  
有些人喜欢总是使用大括号，即使只有一个条目; 这很好，但不是必需的：
```
label1 {
	directive1 arg1
	directive2
}
```

Example in which a directive opens a block:  
指令打开块的示例：
```
label1

directive arg1 {
    subdir arg2 arg3
}
directive arg4
```

Similarly, but in an indented definition body, and with a comment:  
同样，但是在一个缩进的定义体中，并且有一个注释：
```
label1 {
	directive1 arg1
	directive2 arg2 {
	    subdir1 arg3 arg4
	    subdir2
	    # nested blocks not supported
	}
	directive3
}
```