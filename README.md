tir (tEXT iNSERTEr)
===
Contents
===
* [What's 'tir'?](#whats-tir)
* [Install](#install)
* [Syntax](#syntax)
* [What's 'tir'?](#whats-tir)
* [Help](#help)
* [Case examples](#case-examples)
  * [1. Into a HTML file, inserting HTML files](#1-into-a-html-file-inserting-html-files)
  * [2. Multi-File & Multi-Directory & Nesting](#2-multi-file--multi-directory--nesting)
* [Deference of `sed` command](#deference-of-sed-command)
 
What's 'tir'?
===

`tir` is a program for inserting the data of specified file into the specified place of a input file.

Install
===
```
$ sudo make install
```

Syntax
===
```
$ tir <input file> [-o <output file>] [-bw <beginning word>] [-ew <ending word>] [-y] [-makefile] [-h]
```

Help
===

```
$ tir
Usage:
  -o  : Output file
  -bw : Beginning word for inserting
  -ew : Ending word for inserting
        e.g. tir infile.c -o outfile.c -bw '//[tir:begin]' -ew '[tir:end]'
  -h  : Display help
  -y  : To override without asking
  -makefile:
        Output makefile info.
Default words:
  Suffix       | Begin           | End
  ---------------------------------------------
  html htm xml | <!--[tir:begin] | [tir:end]-->
  sh rb py r   | #[tir:begin]    | [tir:end]#
  other(e.g. c)| /*[tir:begin]   | [tir:end]*/
```
Case examples
===
##1. Into a HTML file, inserting HTML files

Command:
```
$ tir ./example1/test.html.tir -y
```
or
```
$ make ex1
```

test.html.tir:
```
<!DOCTYPE html>
<html>
	<head>
		<!--[tir:begin] ref="test_sub.html" convert="urlpaese" [tir:end]-->
	</head>
	<body>
		<!--[tir:begin] 	ref=test sss.html [tir:end]-->
		<!--[tir:begin]ref=test_sss3.html[tir:end]-->
	</body>
</html>
```

test_sub.html:
```
<h1>Hello!!</h1>
```

result(test.html)
```
<!DOCTYPE html>
<html>
	<head>
		<h1>Hello!!</h1>
	</head>
	<body>
		<!--[tir:begin] msg="Can't open a file:test" [tir:end]-->
		<!--[tir:begin] msg="Can't open a file:test_sss3.html" [tir:end]-->
	</body>
</html>
```

Explanations:
 * The contents of specified file(test_sub.hmtl) at `ref` attribute were inserted.
 * No action for invalid attributes(e.g. above `convert`).
 * When a specified file was not exist, `tir` inserts message at `msg` attribute.
 * Output file name became a name that was removed `.tir` suffix from input file name.

##2. Multi-File & Multi-Directory & Nesting

Command:
```
$ createMakefile.sh ./example2
$ cp ./example2/Makefile.txt ./example2/Makefile
$ make -f ./example2/Makefile
```
or
```
$ make ex2
```
test.html.tir:
```
<!DOCTYPE html>
<html>
	<head>
		<!--[tir:begin] ref="test_sub.html" convert="urlpaese" [tir:end]-->
	</head>
	<body>
		<!--[tir:begin]  ref="./sub/sub.html" [tir:end]-->
	</body>
</html>
```

test_sub.html:
```
<h1>Hello!!</h1>
```
sub/sub.html.tir
```
<!--[tir:begin] ref="hey.html" [tir:end]-->
```
sub/hey.html
```
<h1>Hey!</h1>
```
result(test.html)
```
<!DOCTYPE html>
<html>
	<head>
		<h1>Hello!!</h1>

	</head>
	<body>
		<h1>Hey!</h1>
	</body>
</html>
```

Explanations:
 * 'createMakefile.sh'は引数のディレクトリをベースとして、Makefile.txt（上書きを避けるため.txtを付けた）を生成する
 * 

Deference of `sed` command
===
You think that, `tir` command is not necessary because the combinations of `sed` command & shell script are able to give same result?

I answer you that "That's right! Maybe. I don't know the specific method.".

I designed `tir` as complicated descriptions is not necessary.

If you prefer `tir`, please use and improvement(e.g. Pull Requests).
