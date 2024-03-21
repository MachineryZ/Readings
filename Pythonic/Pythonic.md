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

**建议9** 数据交换值的时候不推荐使用中间变量， 为什么python可以写 

~~~python
x, y = y, x
~~~

以上语句在 python中是如何运行的？

- python表达式的计算顺序是从左到右
- 但是遇到表达式赋值的时候表达式右边的操作数先于左边的操作数计算
- expr3, expr4 = expr1, expr2 的计算顺序是：expr1, expr2 -> expr3, expr4
- 1）先计算右侧表达式 y, x 因此在内存中创建元组 (y, x)
- 2）计算左边的值并进行赋值，元组被依次分配给左边的标识符，通过解压缩，元组第一标识符给左边第一元素，以此类推
- 直接用寄存器进行交换

**建议10** 充分利用 Lazy Evaluation 的特性

- 避免不必要的计算，带来性能上的提升。python 中的条件表达式 if x and y 当 x 为 false 的情况下 y 的表达式值将不再计算，所以充分利用该特性
- 节省空间，使得无限循环的数据结构成为可能。最典型的使用延迟计算的例子就是生成器表达式，它仅在每次需要计算的时候才通过 yield 产生所需要的元素。

~~~python
def fib():
	a, b = 0, 1
	while True:
		yield a
		a, b = b, a+b
from itertools import islice
print(list(islice(fib(), 5)))
[0,1,1,2,3]
~~~

**建议11** 理解枚举替代实现的缺陷

**建议12** 不推荐使用 type 来进行类型检查。原因是任意类的实例的 type 返回结果都是 <type 'instance'>，这种情况下使用 type 函数来确定两个变量类型是否相同，结果会非常有问题。

~~~
isinstance(2, float)
isinstance("A", (str,unicode))
isinstance((2,3), (str, list, tuple))
~~~

**建议13** 尽量转换为浮点类型后再做除法。慎用 !=（浮点数精度问题）

**建议14** 警惕 eval() 安全漏洞，eval 本质是用把str类型转换为表达式，如果对象不是信任源，那么应该尽量避免使用 eval，在需要使用 eval 的地方可用安全性更高的 ast.literal_eval 替代

**建议15** 使用enumerate 获取序列迭代的索引和值（list），但是对于字典的遍历，应该使用如下的 iteritems 方法

**建议16** 分清 == 与 is 的适用场景一个是 object identity 一个是 equal，要分清楚。（较小字符串有 stirng interning 字符串驻留机制，会保存同一个 id，这里 is 会判断成 true）

**建议17** 考虑兼容性，尽可能使用 unicode

**建议18** 构建合理的包层次来管理 module，本质上 python 文件都是一个模块，使用模块可以增强代码的可维护性和可重用性。我们需要合理的组织项目的层次来管理模块，这就是 package 发挥功效的地方。

~~~
Package/ __init__.py
	Module1.py
	Module2.py
	Subpackage/ __init__.py
		Module1.py
		Module2.py
~~~

- 直接导入一个包 'import Package'
- 导入子模块或子包 'from Package import Module1, iport Package.Module1'
- 在包对应的目录下包含有 '\__init__.py' 的文件，这个文件的作用就是使包和普通目录区分；其次可以在该文件中申明模块级别的 import 语句
- 如果要 import 包 Package 下 Module1 中的类 Test，当 \__init__.py 文件为空的时候需要使用完整的路径来申明 import 语句 'from Package.Module1 import Test'
- 但如果在 \__init__.py 文件中添加 from Module1 import Test 语句，则可以直接使用 from Package import Test 来导入类 Test
- \__init__.py 文件还有一个作用就是通过在该文件中定义 \_all\_ 变量，控制需要导入的子包护着模块，例如在 Package 目录下的 \_\_init\_\_.py 中添加：

~~~
__all__ = ['Module1', 'Module2', 'Subpackage']

>>> from Package import *
>>> dir()
['Module1', 'Module2', 'Subpackage', '__builtins__', '__doc__', '__name__', '__package__']
~~~

一个可供参考的 python 项目结构：

~~~
ProjectName/
|---README
		|---LICENSE
		|---setup.py
		|---requirements.txt
		|		|---__init__.py
		|		|---core.py
		|		|---helpers.py
		|---docs/
				|		|---conf.py
				|		|---index.rst
		|---bin/
		|---package/
		|		|---__init__.py
		|		|---subpackage/
		|		|---...
		|---tess/
		|		|---test_basic.py
		|		|---test_advanced.py
~~~

**建议19** 有节制的使用 from import 语句

- 一般情况下尽量优先使用 import a 形式，需要访问a中的 B 则使用 a.B
- 有节制的使用 from a import B
- 尽量避免使用 from a import *，因为这样会污染命名空间

**建议20** 优先使用 absolute import 来导入模块

**建议21** i += 1 不等于 ++i，++i 会被操作解释为 +(+i)

**建议22** 使用 with 自动关闭资源

**建议24** 遵循异常处理的几点基本原则 try-except- except- else-finally

- except 是当上一个 except 发生异常时进行下一个except
- else 是没有任何异常时发生执行
- finally 不管有没有异常发生都有执行
- 单独使用 except 会捕获包括 systemexit，keyboardinterrupt 等在内的各种异常 从而掩盖程序真正发生异常的原因

**建议27** 使用 join 的方式连接字符串，速度快很多，原理：

- 如果用 +，那么执行顺序是分配新的内存空间，每执行一次 + 都会分配一次新的内存空间。实际上总的字符串连接时间复杂度接近 O(N^2)（因为最初的字符串被扫描了两次）
- 如果用 join，那么实际上是一开始就计算所需要的总的内存空间，然后一次性申请所需内存中的每一个元素，复制到内存中去，所以 join 的时间复杂度是 O(N)

**建议28** 格式化字符串时尽量使用 .format 方式而不是 %

**建议31** 函数传参既不是传值也不是传引用，可变对象传引用、不可变对象传值，也是不准确的。python的参数传递机制到底是怎么样的，首先理解 python 中的赋值和我们理解的 赋值并不一样。"a=5,b=a,b=7"

- c++ 的 b = a 是将内存申请一块并将a的值赋值到内存中，执行 b=7则是将b对应的值从5修改到7
- 但是在 python 中 b=a 操作会使得b和a引用同一个对象
- 最终回答是：既不是传值也不是传引用。正确的说法是传对象或者传对象的引用。传入参数的过程中将整个对象传入，对可变对象的修改在函数外部以及内部都可以见，调用者和被调用者之间共享对象；不可变对象，不能被真正修改，因此都是通过生成新的对象然后来赋值实现的

**建议32** 警惕默认参数潜在的问题

~~~python
def appendtest(newitem, lista = []):
  print(id(lista))
  lista.append(newitem)
  print(id(lista))
  return lista

appendtest('a')
appendtest('a')

~~~

最终会输出 ['a', 'a']，这是因为解释器执行 def 的时候，默认参数也会被计算，并存在函数的 .func\_defaults 属性中。由于python中函数参数传递的是对象，可变对象在调用者和被调用者之间共享，所以会出现以上bug

**建议33** 慎用变长参数，python支持可变长度的参数列表，可以通过在函数定义的时候使用 *args 和 **kwargs 这两个特殊语法来实现

~~~python
def set_axis(x, y, xlabel='x', ylabel='y', *args, **kwargs):
	pass
	
set_axis(2, 3, "test", "test2", "test3", my_kwargs="test3")
set_axis(2, 3, my_kwarg="test3")
set_axis(3, 3, "test1", my_kwargs="test3")
set_axis("test1", "test2", xlabel="new_x", ylabel="new_y", my_kwarg="test3")
set_axis(2, "test1", "test2", ylabel="new_y", my_kwarg="test3")
~~~

以上所有调用都是合法的！首先满足普通参数，然后默认参数。使用可变长度参数，使用过于灵活。以下情况下使用：

- 实现函数多台或者在继承情况下子类调用父类的某些方法
- 如果参数数目不确定，读取一些配置文件中的值进行全局变量初始化
- 为函数添加一个装饰器

**建议34** 深入理解 str 和 repr 的区别

- 都是将 python 中的对象转换为字符串
- 解释其中直接输入 a 调用 repr 参数，print 调用 str 函数
- str主要面向用户，目的是可读性；repr 面向的是python解释器、开发人员，常作为debug用途
- 一般来说类内都应该定义 \__repr\_\_ 方法，用户实现时候最好保证其返回值可以用 eval 还原

**建议35** 分清 staticmethod 和 classmethod 的适用场景

**建议38** 使用 copy 模块深拷贝对象

**建议39** 使用 counter 进行计数统计，除了遍历一边set、list这种iterator类型的，还有没有更佳 pythonic 的做法？使用 Counter 类，他是属于字典类的子类，是一个容器对象，主要用来统计散列对象，支持运算符操作。

 ~~~python
 from collections import Counter
 some_data = ['a', '2', 2, 4, 5, '2', 'a', 'b', 'a']
 print(Counter(some_data))
 ~~~

**建议40** 深入掌握 ConfigParser，配置文件的意义在于用户不需要修改代码就可以改变应用程序的行为，python 有一个标准库来支持，也就是 ConfigParser

**建议41** 使用 argparse 处理命令行参数，除此之外标准库中留下的 getopt、optparse 和 argparse 就是证明。其中 getopt 是类似



| type               | Aix_fORGE             | mh                    |
| ------------------ | --------------------- | --------------------- |
| 数据类型           | Feather               | 自定义的 npcbuf       |
| 可解释性模块       | AIx_forge_Interpreter | /                     |
| 多机多卡分布式训练 | /                     | /                     |
| 模型接口           | DL + Tree             | Only DL               |
| 数据采用           | Graph（进化版） + Seq | Graph（普通版） + Seq |
| 数据分析           | AIx_forge_Analyzer    | /（需要自己实现）     |
|                    |                       |                       |
|                    |                       |                       |
|                    |                       |                       |













































