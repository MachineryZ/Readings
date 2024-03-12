Pythonic 就是对 python 愈发本身的充分发挥，写出来的代码带着 python 味道，而不是看着像 C语言代码，或者 Java 代码



**建议1** 理解 pythonic 概念

- 灵活使用迭代器

~~~python
length = len(alist)
i = 0
while i < length:
	do_sth_with(alist[i])
	i += 1
	
# Pythonic:
for i in alist:
  do_Sth_with(alist[i])
~~~

- 安全关闭文件描述符使用 with 语句

~~~python
with open(path, 'r') as f:
	do_sth_with(f)
~~~

- python 包和模块结构日益规范化，现在的库或框架跟碎了一下潮流：
  - 包和模块的命名采用小写、单数形式，而且短小
  - 包通常仅作为命名空间，如只包含空的 _____init_.py 文件

**建议2** 编写 pythonic 代码，避免劣化代码，比如避免只用大小写来区分不同的对象、避免使用混淆的名称（内建名称list element等，使用o和0混淆，1和L混淆，不要害怕过长的变脸名。

全面掌握python提供的所有特性，包括语言特性和库特性；学习每个python新版本提供的心特性和新支持；深入学习业界公认的比较pythonic的代码，比如 flask、gevent和requests等

python 风格检查程序 PEP8，包含了对代码布局、注释、命名规范的等方面的要求，在代码中遵循这些原则有利于编写 pythonic 的代码（PEP8 不是唯一的风格检测程序，类似的还有 Pychecker、Pylint、Pyflakes等）



**建议3** 理解python 与 C 的不同，虽然 python底层是 C，但是风格和思维上和 C 非常不一样

- 缩进和花括号，不要混用 tab 和 空格
- 单引号和双引号，单引号表示字符，双引号表示字符串
- 三元操作符 X if Y else Z
- switch case

**建议4** 在代码中适当添加注释，python 中有三种形式的代码注释：块注释、行注释以及文档注释 docstring

- 使用块注释或者行注释的时候仅仅注释哪些复杂的操作和算法，还有可能难以理解的代码
- 使用块或者行注释的时候仅仅注释哪些浮躁的操作、算法
- 注释和代码隔开一定的距离
- 给外部可访问的函数和方法添加文档注释，注释要清楚的描述方法的功能，并对参数、返回值、以及可能发生的异常进行说明，是的外部调用它的人员仅仅看docstring就能正确使用

~~~python
def FuncName(parameter1, parameter2,...):
  """
  Describe what this function does:
  such as "Find whether the special stirng is in the queue or not"
  Args:
  	parameter1: parameter type, what is this parameter used for.
  	parameter2: parameter type, what is this parameter used for
 	Returns:
 		return type, reutnr value
  """
~~~

- 推荐在头文件中包含 copyright 申明、模块描述等，如有必要可以考虑加入作者信息以及变更记录

~~~python
"""
Licensed Material - Property of CorpA
(C) Copyright A Corp. 1999, 2011 All Rights Reserved
Copy Right statement and purpose...
----------------------------------------
File Name: comments.py
Description: description what the main function of this file

Author: Author name
Change Activity:
	list the change activity and time and author information
"""
~~~

















































