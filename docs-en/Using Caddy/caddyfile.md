Caddyfile Syntax
This page describes the syntax of the Caddyfile. If it is your first time writing a Caddyfile, try the Caddyfile primer tutorial instead. This page is not beginner-friendly; it is technical and kind of boring.

Although this article is verbose, the Caddyfile is designed to be easily readable and writable by humans. You will find that it is easy to remember, not cumbersome, and flows off the fingers.

The term "Caddyfile" often refers to a file, but more generally means a blob of Caddy configuration text. A Caddyfile can be used to configure any Caddy server type: HTTP, DNS, etc. The basic structure and syntax of the Caddyfile is the same for all server types, but semantics change. Because of this variability, this document treats the Caddyfile only as the generic configuration syntax as it applies to all server types. Caddyfile documentation for specific types may be found within their respective docs. For instance, the HTTP server documents the semantics of its Caddyfile.

Topics
File Format & Encoding
Lexical Syntax
Structure
Labels
Directives
Environment Variables
Import
Examples
File Format & Encoding
The Caddyfile is plain Unicode text encoded with UTF-8. Each code point is distinct; specifically, lowercase and uppercase characters are different. A leading byte order mark (0xFEFF), if present, will be ignored.

Lexical Syntax
A token is a sequence of whitespace-delimited characters in the Caddyfile. A token that starts with quotes " is read literally (including whitespace) until the next instance of quotes " that is not escaped. Quote literals may be escaped with a backslash like so: \". Only quotes are escapable. “Smart quotes” are not valid as quotes.

Lines are delimited with the \n (newline) character only. Carriage return \r is discarded unless quoted. Blank, unquoted lines are allowed and ignored.

Comments are discarded by the lexer. Comments begin with an unquoted hash # and continue to the end of the line. Comments may start a line or appear in the middle of a line as part of an unquoted token. For the purposes of this document, commented and blank lines are no longer considered.

Tokens are then evaluated by the parser for structure.

Structure
A Caddyfile has no global scope. The most global unit of the Caddyfile is an entry. An entry consists of a list of labels and a definition associated with those labels. A label is a string identifier, and a definition is a body (one or more lines) of tokens grouped together in a block:

list of labels
definition (block)
A Caddyfile with only one entry may consist simply of the label line(s) followed by the definition on the next line(s), as shown above. However, a Caddyfile with more than one entry must enclose each definition in curly braces { }. The opening curly brace { must be at the end of the label line, and the closing curly brace } must be the only token on its line:

list of labels {
definition (block)
}
list of labels {
definition (block)
}
Consistent tab indentation is encouraged within blocks enclosed by curly braces.

The first line of a Caddyfile is always a label line. Comment lines, empty lines, and import lines are the exceptions.

Labels
Labels are the only tokens that appear outside of blocks (with one exception being the import directive). A label line may have just one label:

label
or several labels, separated by spaces:

label1 label2 ...
If many labels are to head a block, the labels may be suffixed with a comma. A comma-suffixed label may be followed by a newline, in which case the next line will be considered part of the same line:

label1,
label2
Mixing of these patterns is valid (but discouraged), as long as the last label of the line has a comma if the next line is to continue the list of labels:

label1 label2,
label3, label4,
label5
A definition with multiple labels is replicated across each label as if they had been defined separately but with the same definition.

Directives
The body of the definition follows label lines. The first token of each line in a definition body is a directive. Every token after the directive on the same line is an argument. Arguments are optional:

directive1
directive2 arg1 arg2
directive3 arg3
Commas are not acceptable delimiters for arguments; they will be treated as part of the argument value. Arguments are delimited solely by same-line whitespace.

Directives may span multiple lines by opening a block. Blocks are enclosed by curly braces { }. The opening curly brace { must be at the end of the directive's first line, and the closing curly brace } must be the only token on its line:

directive {
    ...
}
Within a directive block, the first token of each line may be considered a subdirective or property, depending on how it is used (other terms may be applied). And as before, they can have arguments:

directive arg1 {
    subdir arg2 arg3
    ...
}
Subdirectives cannot open new blocks. In other words, nested directive blocks are not supported. If a directive block is empty, the curly braces should be omitted entirely.

Environment Variables
Any token (label, directive, argument, etc.) may contain or consist solely of an environment variable, which takes the Unix form or Windows form, enclosed in curly braces { } without extra whitespace:

label_{$ENV_VAR_1}
directive {%ENV_VAR_2%}
Either form works on any OS. A single environment variable does not expand out into multiple tokens, arguments, or values.

Import
The import directive is a special case, because it can appear outside a definition block. The consequence of this is that no label can take on the value of "import".

Where an import line is, that line will be replaced with the contents of the imported file, unmodified. See the import docs for more information.

Examples
A very simple Caddyfile with only one entry:
label1

directive1 argument1
directive2
Appending the prior example with another entry introduces the need for curly braces:
label1 {
	directive1 arg1
	directive2
}
label2, label3 {
	directive3 arg2
	directive4 arg3 arg4
}
Some people prefer to always use braces even if there's just one entry; this is fine, but unnecessary:
label1 {
	directive1 arg1
	directive2
}
Example in which a directive opens a block:
label1

directive arg1 {
    subdir arg2 arg3
}
directive arg4
Similarly, but in an indented definition body, and with a comment:
label1 {
	directive1 arg1
	directive2 arg2 {
	    subdir1 arg3 arg4
	    subdir2
	    # nested blocks not supported
	}
	directive3
}