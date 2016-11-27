本文给出主Python版本标准库的编码约定。CPython的C代码风格参见PEP7。

本文和PEP 257(文档字符串标准)改编自Guido最初的《Python Style Guide》, 并增加了Barry的GNU Mailman Coding Style Guide的部分内容。

本文会随着语言改变等而改变。

许多项目都有自己的编码风格指南，冲突时自己的指南为准。

# 一致性考虑

Guido的关键点之一是：代码更多是用来读而不是写。本指南旨在改善Python代码的可读性，即PEP 20所说的“可读性计数"(Readability counts)。

风格指南强调一致性。项目、模块或函数保持一致都很重要。

最重要的是知道何时不一致, 有时风格指南并不适用。当有疑惑时运用你的最佳判断，参考其他例子并多问！

特别注意：不要因为遵守本PEP而破坏向后兼容性！

部分可以违背指南情况：

*   遵循指南会降低可读性
*   与周围其他代码不一致
*   代码在引入指南完成，暂时没有理由修改
*   旧版本兼容

# 代码布局

## 缩进

每级缩进用4个空格

括号中使用垂直隐式缩进或使用悬挂缩进。后者应该注意第一行要没有参数，后续行要有缩进


Yes:

    # 对准左括号
    foo = long_function_name(var_one, var_two,
                             var_three, var_four)

    # 不对准左括号，但加多一层缩进，以和后面内容区别。
    def long_function_name(
            var_one, var_two, var_three,
            var_four):
        print(var_one)

    # 悬挂缩进必须加多一层缩进.
    foo = long_function_name(
        var_one, var_two,
        var_three, var_four)

No:

    # 不使用垂直对齐时，第一行不能有参数。
    foo = long_function_name(var_one, var_two,
        var_three, var_four)

    # 参数的缩进和后续内容缩进不能区别对待。
    def long_function_name(
        var_one, var_two, var_three,
        var_four):
        print(var_one)

4个空格的规则是对续行可选的。

    # 悬挂缩进不一定是4个空格
    foo = long_function_name(
      var_one, var_two,
      var_three, var_four)

if语句跨行时，两个字符关键字(比如if)加上一个空格，再加上左括号构成了很好的缩进。后续行暂时没有规定，至少有如下三种格式，建议使用第3种。

    # 没有额外缩进，不是很好看，个人不推荐.
    if (this_is_one_thing and
        that_is_another_thing):
        do_something()

    # 添加注释
    if (this_is_one_thing and
        that_is_another_thing):
        # Since both conditions are true, we can frobnicate.
        do_something()

    # 额外添加缩进,推荐。
    # Add some extra indentation on the conditional continuation line.
    if (this_is_one_thing
            and that_is_another_thing):
        do_something()

右边括号也可以另起一行。有两种格式，建议第2种。

    # 右括号不回退，个人不推荐
    my_list = [
        1, 2, 3,
        4, 5, 6,
        ]
    result = some_function_that_takes_arguments(
        'a', 'b', 'c',
        'd', 'e', 'f',
        )

    # 右括号回退
    my_list = [
        1, 2, 3,
        4, 5, 6,
    ]
    result = some_function_that_takes_arguments(
        'a', 'b', 'c',
        'd', 'e', 'f',
    )

## 空格或Tab?

空格是首选的缩进方法。

Tab仅仅在已经使用tab缩进的代码中为了保持一致性而使用。

Python 3中不允许混合使用Tab和空格缩进。

Python 2的包含空格与Tab和空格缩进的应该全部转为空格缩进。

Python2命令行解释器使用-t选项时有非法混合Tab和空格的情况会告警。当使用-tt警告提升为错误。强烈推荐这些选项！另外个人推荐pep8和autopep8模块。

## 最大行宽

限制所有行的最大行宽为79字符。

文本长块，比如文档字符串或注释，行长度应限制为72个字符。

在代码回顾时，可以让编辑器以side by side 的方式打开几个文件，舒服而完整的看到代码的改变。

多数工具默认的续行功能会破坏代码结构，使它更难理解。让编辑器在自动续行时，对超过80个字符加以提醒符是必要的。一些web编辑器可能根本不具备动态换行功能。

一些团队强烈希望更长的行宽。如果能达成一致，可以从从80提高到100个字符(最多99个字符)增加了标称线的长度，不过依旧建议文档字符串和注释保持在72的长度。

Python标准库比较保守，限制行宽79个字符(文档字符串/注释72）。

续行的首选方法是使用小括号,中括号和大括号,进行隐式换行，在续行时优先于反斜杠使用。

反斜杠仍可能在适当的时候,比如with语句中：

    with open('/path/to/some/file/you/want/to/read') as file_1, \
         open('/path/to/some/file/being/written', 'w') as file_2:
        file_2.write(file_1.read())

类似的还有assert。

## 应该在二元运算符之前还是之后换行？

多年来我们被推荐在二元操作符后换行，但是这在两个方面伤害了可读性：运算符倾向于散步在不同行上，每个运算符都与它的运算数远离。我们的眼睛需要去做额外的工作把他们凑到一起。

    # No: 操作符跟他们的操作数远离
    income = (gross_wages +
              taxable_interest +
              (dividends - qualified_dividends) -
              ira_deduction -
              student_loan_interest)

为了解决这一可读性问题，数学家和他们的出版商遵循相反的约定。

以下的数学运算传统通常可以使得结果更加具有可读性:

    # Yes: 比较容易匹配操作符和操作数
    income = (gross_wages
              + taxable_interest
              + (dividends - qualified_dividends)
              - ira_deduction
              - student_loan_interest)

在Python代码中,允许打破之前或之后一个二元运算符的规则,只要与当前惯例是一致的。新代码建议使用数学家的风格。

## 空行

顶层函数和类的定义由两个空行分割。

类的方法定义用单个空行分割。

额外的空行可以必要的时候用于分割不同的函数组。在一堆相关的单行函数组之间可以省略空行（比如一堆伪实现的函数）

额外的空行可以必要的时候在函数中用于分割不同的逻辑块。

Python接受contol-L(即^L)换页符作为空格。许多工具 视这个字符为页面分割符,因此在你的文件中,可以用他们来为相关片段(sections)分页。注意，一些编辑器和基于Web的代码查看器可能不能识别control-L是换页，将显示另外的字形。

## 源文件编码

在核心Python发布的代码应该总是使用UTF-8(ASCII在Python 2)。

ASCII文件(Python 2)或UTF-8(Python 3)不应有编码声明。

标准库中非默认的编码应仅用于测试，或当注释或文档字符串,比如包含非ASCII字符的作者姓名，否则使用\ x，\ u，\ U或\ N转义是在字符串文字中包含非ASCII数据的首选方式。

Python 3.0及以后版本，PEP 3131可供参考，部分内容如下：在Python标准库必须使用ASCII标识符，并尽量只使用英文字母。此外字符串和注释也必须用ASCII。唯一的例外是：（a）测试非ASCII的功能，和（b）作者的名字不是拉丁字母。

对于Python 3.0及更高版本，标准库（见PEP 3131）规定了以下策略：Python标准库中的所有标识符必须使用ASCII唯一标识符，并且应尽可能使用英语单词（在许多情况下，缩写和技术名词使用不是英语的术语）。 此外，字符串字面值和注释也必须为ASCII。 唯一的例外是（a）测试非ASCII特征的测试用例，和（b）作者的名字。 名称不是基于拉丁字母的作者必须提供他们名字的拉丁音译。

## 导入

*   导入在单独行

	Yes：

    	import os
    	import sys
    	from subprocess import Popen, PIPE

	No:

    	import sys, os

*   导入始终在文件的顶部，在模块注释和文档字符串之后，在模块全局变量和常量之前。

	 导入顺序如下：
	 
	     标准库
	     相关的第三方库
	     本地库
	     
	各组的导入之间要有空行。
	
*   推荐绝对路径导入，因为它们通常更可读，而且当导入系统配置不正确时，往往是表现更好的（或至少提供更好的错误消息）：

    	import mypkg.sibling
    	from mypkg import sibling
    	from mypkg.sibling import example

	在处理复杂的包布局，导致绝对路径不必要的冗长的时候，也可以使用相对路径：

    	from . import sibling
    	from .sibling import example

	Python 3中已经禁止隐式的相对导入。

*   导入类的方法：

	    from myclass import MyClass
	    from foo.bar.yourclass import YourClass

	如果和本地名字有冲突：

	    import myclass
	    import foo.bar.yourclass
	   
	然后使用myclass.MyClass和foo.bar.yourclass.YourClass

*   该避免通配符导入（从<module> import *），因为它们使得不清楚名称空间中存在哪些名称，使读者和许多自动化工具混淆。 对于通配符导入，有一个可讨论的情况，即重新发布内部接口作为公共API的一部分（例如，使用来自可选加速器模块的定义覆盖接口的纯Python实现，以及将哪些定义覆盖不是预先知道的）。

	当以这种方式重新发布名称时，有关公共和内部接口的以下准则仍然适用

## 模块级dunder名

模块级别“dunders”（即具有两个前导和两个尾随下划线的名称），例如__all__，__author__，__version__等，应放在模块docstring之后，在任何import语句之前，除了__future__导入。 Python强制future-imports必须在除了docstrings之外的任何其他代码之前出现在模块中:

	"""This is the example module.
	
	This module does stuff.
	"""

	from __future__ import barry_as_FLUFL

	__all__ = ['a', 'b', 'c']
	__version__ = '0.1'
	__author__ = 'Cardinal Biggles'

	import os
	import sys

# 字符串引用

Python中单引号字符串和双引号字符串都是相同的。pep在此不做推荐。坚持其中一个规则就好了。注意尽量避免在字符串中的反斜杠以提高可读性。

根据PEP 257, 三个引号都使用双引号。

# 表达式和语句中的空格

## 强制要求

在以下情况避免多余的空格出现

*   括号里边避免空格

	Yes:
	
	    spam(ham[1], {eggs: 2})
	
	No:
	
	    spam( ham[ 1 ], { eggs: 2 } )

*   逗号，冒号，分号之前避免空格

	Yes:
	
	    if x == 4: print x, y; x, y = y, x
	
	No:
	
	    if x == 4 : print x , y ; x , y = y , x

*   在切片中冒号像一个二元操作符，冒号两侧应该有相同数量的空格（把它看作最低优先级的操作符）。 在一个扩展切片中，两个冒号必须有相等数量的空格。例外是，当一个切片参数被省略时，该空格也要被省略。

	Yes:
	
	    ham[1:9], ham[1:9:3], ham[:9:3], ham[1::3], ham[1:9:]
	    ham[lower:upper], ham[lower:upper:], ham[lower::step]
	    ham[lower+offset : upper+offset]
	    ham[: upper_fn(x) : step_fn(x)], ham[:: step_fn(x)]
	    ham[lower + offset : upper + offset]
	
	No:
	
	    ham[lower + offset:upper + offset]
	    ham[1: 9], ham[1 :9], ham[1:9 :3]
	    ham[lower : : upper]
	    ham[ : upper]

*   函数调用的左括号之前不能有空格

	Yes:
	
	    spam(1)
	    
	
	No:
	
	    spam (1)
	  
*	索引和切片操作的左中括号前不能有空格

	Yes：
	
		dct['key'] = lst[index]
		
	NO：
	
		dct ['key'] = lst [index]

*   赋值等操作符前后不能因为对齐而添加多个空格

	Yes:
	
	    x = 1
	    y = 2
	    long_variable = 3
	
	No:
	
	    x             = 1
	    y             = 2
	    long_variable = 3

## 其他建议

*	避免尾随空格。因为它通常是无形的,它可能会让人很困惑:如一个反斜杠,后跟一个空格和一个换行符不算作一行延续标记。有些编辑器不保护它，并且许多项目(如CPython本身)具有拒绝它的预提交钩子。

*   始终在这些二元运算符两边放置一个空格: 赋值（=）、符合赋值 ( += , -=等)、比较( == , < , > , != , <> , <= , >= , in , not in , is , is not )、布尔( and , or , not )。

*   优先级高的运算符或操作符的前后不建议有空格。

	Yes:
	
	    i = i + 1
	    submitted += 1
	    x = x*2 - 1
	    hypot2 = x*x + y*y
	    c = (a+b) * (a-b)
	
	No:
	
	    i=i+1
	    submitted +=1
	    x = x * 2 - 1
	    hypot2 = x * x + y * y
	    c = (a + b) * (a - b)

*   关键字参数和默认值参数的前后不要加空格

	Yes:
	
	    def complex(real, imag=0.0):
	        return magic(r=real, i=imag)
	
	No:
	
	    def complex(real, imag = 0.0):
	        return magic(r = real, i = imag)

*   函数注释应该对冒号使用正常规则，并且，如果存在 -> ，两边应该有空格。

	Yes:
	
		def munge(input: AnyStr): ...
		def munge() -> AnyStr: ...
	
	No:
	
		def munge(input:AnyStr): ...
	   	def munge()->PosInt: ...
	    
*	结合一个参数注释和默认值时,在 = 符号两边使用空格(仅仅当那些参数同时有注释和一个默认值时)。

	Yes:
	
		def munge(sep: AnyStr = None): ...
		def munge(input: AnyStr, sep: AnyStr = None, limit=1000): ...
			
	No:
	
		def munge(input: AnyStr=None): ...
		def munge(input: AnyStr, limit = 1000): ...


*   通常不推荐复合语句(Compound statements: 多条语句写在同一行)。

	Yes:
	
	    if foo == 'blah':
	        do_blah_thing()
	    do_one()
	    do_two()
	    do_three()
	
	No:
	
	    if foo == 'blah': do_blah_thing()
	    do_one(); do_two(); do_three()

*   尽管有时可以将if/for/while和一小段主体代码放在一行，但在一个有多条子句的语句中不要如此。避免折叠长行!

	No:
	
	    if foo == 'blah': do_blah_thing()
	    for x in lst: total += x
	    while t < 10: t = delay()
	
	No:
	
	    if foo == 'blah': do_blah_thing()
	    else: do_non_blah_thing()
	
	    try: something()
	    finally: cleanup()
	
	    do_one(); do_two(); do_three(long, argument,
	                                list, like, this)
	
	    if foo == 'blah': one(); two(); three()

# 注释

与代码自相矛盾的注释比没注释更差。修改代码时要优先更新注释！

注释是完整的句子。如果注释是一个短语或句子,首字母应该大写, 除非他是一个以小写字母开头的标识符(永远不要修改标识符的大小写)。

如果注释很短，可以省略末尾的句号。注释块通常由一个或多个段落组成。段落由完整的句子构成且每个句子应该以点号(后面要有两个空格)结束。

你应该在句末的句号后使用两个空格。

用英语书写时,断词和空格是可用的。

非英语国家的程序员请用英语书写你的注释，除非你120%确信代码永远不会被不懂你的语言的人阅读。

## 注释块

注释块通常应用在代码前，并和这些代码有同样的缩进。每行以 '# '(除非它是注释内的缩进文本，注意#后面有空格)。

注释块内的段落用仅包含单个 '#' 的行分割。

## 行内注释

慎用行内注释(Inline Comments)。 

行内注释是和语句在同一行，至少用两个空格和语句分开。行内注释不是必需的，重复罗嗦会使人分心。不要这样做：

    x = x + 1           # Increment x

但有时很有必要:

	 x = x + 1            # Compensate for border

## 文档字符串

文档字符串的标准参见：PEP 257。

*	为所有公共模块、函数、类和方法书写文档字符串。非公开方法不一定有文档字符串，建议有注释(出现在 def 行之后)来描述这个方法做什么。

*	更多参考：PEP 257 文档字符串约定。注意结尾的 """ 应该单独成行，例如：

		"""Return a foobang
		
		Optional plotz says to frobnicate the bizbaz first.
		"""

*	单行的文档字符串，结尾的 """ 在同一行。

# 命名约定

Python库的命名约定有点混乱，不可能完全一致。但依然有些普遍推荐的命名规范的。新的模块和包 (包括第三方的框架) 应该遵循这些标准。对不同风格的已有的库，建议保持内部的一致性。

## 最重要的原则

用户可见的API命名应遵循使用约定而不是实现。

## 描述：命名风格

有多种命名风格：

*	b(单个小写字母)

*	B(单个大写字母)

*	lowercase(小写串)

*	lower_case_with_underscores(带下划线的小写)

*	UPPERCASE(大写串)

*	UPPER_CASE_WITH_UNDERSCORES(带下划线的大写串)

*	CapitalizedWords(首字母大写的单词串或驼峰缩写）

	注意: 使用大写缩写时，缩写使用大写字母更好。故 HTTPServerError 比HttpServerError 更好。

*	mixedCase(混合大小写，第一个单词是小写)

*	Capitalized_Words_With_Underscores（带下划线，首字母大写，丑陋）

还有一种风格使用短前缀分组名字。这在Python中不常用， 但出于完整性提一下。例如，os.stat()返回的元组有st_mode, st_size, st_mtime等等这样的名字(与POSIX系统调用结构体一致)。

X11库的所有公开函数以X开头, Python中通常认为是不必要的，因为属性和方法名有对象作前缀，而函数名有模块名为前缀。

下面讲述首尾有下划线的情况:

*	_single_leading_underscore:(单前置下划线): 弱内部使用标志。 例如"from M import * 不会导入以下划线开头的对象。

*	single_trailing_underscore_(单后置下划线): 用于避免与 Python关键词的冲突。 例如：

		Tkinter.Toplevel(master, class_='ClassName')

*	__double_leading_underscore(双前置下划线): 当用于命名类属性，会触发名字重整。 (在类FooBar中，__boo变成 _FooBar__boo)。

*	__double_leading_and_trailing_underscore__(双前后下划线)：用户名字空间的魔法对象或属性。例如:__init__ , __import__ or __file__，不要自己发明这样的名字。

## 规范：命名约定

### 避免采用的名字

决不要用字符'l'(小写字母el)，'O'(大写字母oh)，或 'I'(大写字母eye) 作为单个字符的变量名。

一些字体中，这些字符不能与数字1和0区别。用'L' 代替'l'时。

### 包和模块名

模块名要简短，全部用小写字母，可使用下划线以提高可读性。包名和模块名类似，但不推荐使用下划线。

模块名对应到文件名，有些文件系统不区分大小写且截短长名字，在 Unix上不是问题，但当把代码迁移到 Mac、Windows 或 DOS 上时，就可能是个问题。当然随着系统的演进，这个问题已经不是经常出现。

另外有些模块底层用C或C++ 书写，并有对应的高层Python模块，C/C++模块名有一个前置下划线 (如：_socket)。

### 类名

遵循CapWords。

接口需要文档化并且可以调用时，可能使用函数的命名规则。

注意大部分内置的名字是单个单词（或两个），CapWord只适用于异常名称和内置的常量。

###  类型变量名

类型变量名由 pep 484 引入，一般使用CapWords约定，更倾向于使用短的名字：T，AnyStr，Num。推荐添加后缀_co或_contra用来声明协变或逆变行为相应的变量。

	from typing import TypeVar

	VT_co = TypeVar('VT_co', covariant=True)
	KT_contra = TypeVar('KT_contra', contravariant=True)

### 异常名

因为异常应该是一个类，类的规范也适用这里。无论怎样，你应该在异常名中使用后缀"Error"(如果实际是一个错误).

### 全局变量名

(让我们希望这些变量打算只被用于模块内部) 这些约定与那些用于函数的约定差不多.

对设计为通过 "from M import " 来使用的模块，应采用 __all__ 机制来防止导出全局变量；或使用加前缀的旧规则，在那些不想被导入的全局变量前加一个下划线（代表这些全局变量是模块间非公开的）


### 函数名

函数名应该为小写，必要时可用下划线分隔单词以增加可读性。 

mixedCase(混合大小写)仅被允许用于兼容性考虑(如: threading.py)。

### 函数和方法的参数

实例方法第一个参数是 'self'。

类方法第一个参数是 'cls'。

如果一个函数的参数名与保留关键字冲突，最好是为参数名添加一个后置下划线而不是使用缩写或错误的拼写。因此class_ 比clss好。（也许使用同义词来避免更好。）

### 方法名和实例变量

同函数命名规则：小写以下划线分开以提高可读性

非公开方法和实例变量增加一个前置下划线。

为了避免和子类命名冲突，使用两个前导下划线调用Python的名称改编规则。

Python用类名改编这些名字：类Foo属性名为__a，不能以 Foo.__a访问。(执著的用户还是可以通过Foo._Foo__a。) 通常双前置下划线只能被用来避免与子类的属性发生命名冲突。

注意：关于__names的使用存在一些争论（见下文）。

### 常量

常量通常在模块级定义,由大写字母用下划线分隔组成。比如括MAX_OVERFLOW和TOTAL。

### 继承设计

考虑类的方法和实例变量(统称为属性)是否公开。如果有疑问，选择不公开；把其改为公开比把公开属性改为非公开要容易。

公开属性可供所有人使用，并通常向后兼容。非公开属性不给第三方使用、可变甚至被移除。

这里不使用术语"private"， Python中没有属性是真正私有的。

另一类属性是子类API(在其他语言中通常称为 "protected")。 一些类被设计为基类，可以扩展和修改。在设计这样的类的时候，要注意明确哪些属性是公开的，哪些是API子类的一部分，哪些是真正只在你的基类中使用。

清楚这些之后，以下是 Pythonic 的指南：

*	公开属性应该没有前导下划线。

*	如果公开属性名和保留关键字冲突，可以添加后置下划线。这比缩写或拼写错误更可取。（然而，尽管有这样的规定，对于任何类的变量或参数，特别是类方法的第一个参数，‘cls’是首选的拼写方式）

*	简单的公开数据属性，最好只公开属性名，没有复杂的访问/修改方法。记住，Python为未来增强提供了一条简单的途径，你应该发现简单的数据属性需要增加功能行为。在这种情况下，使用属性（properties）来隐藏简单数据属性访问语法后面的功能实现。

*	如果确定你的类会被子类化，并有不想子类使用的属性，考虑用两个前导下划线无后置下划线来命名它们。这将调用Python的名称改编算法，类名将被改编为属性名。当子类不无意间包括相同的属性名时，这有助于帮助避免属性名冲突。

## 公共和内部接口

任何向后兼容性保证只适用于公共接口。因此，重要的是用户能够清楚地区分公开和内部接口。

文档接口被认为是公开的，除非文档明确声明他们是临时或内部接口，免除通常的向后兼容保证。所有非文档化的接口应假定为内部接口。

为了更好地支持内省，模块应该使用__all__属性，显示声明他们公开API的名字，__all__设置为一个空列表表示该模块没有公开API。

即使__all__设置的适当，内部接口（包，模块，类，函数，属性或者其它名字）仍应以一个前导下划线作前缀。

内部接口要有前置下划线。

如果一个接口包含任何命名空间（包，模块，或类）被认为是内部的，那么该接口被认为是内部的

导入名称应视为实现细节。其它模块必须不依赖间接访问这个导入名，除非他们是一个明确的记录包含模块的API的一部分，如os.path中或包的__init__模块，从子模块暴露了功能。

# 编程建议

*	考虑多种Python实现(PyPy, Jython, IronPython,Pyrex, Psyco, 等等)。

	例如，CPython对a+=b或a=a+b等语句有高效的实现，但在Jython中运行很慢，尽量改用.join()。
	
*	None比较用'is'或'is not'，不要用等号。

	注意"if x is not None" 与"if x" 的区别。

*	用"is not"代替"not ... is"。前者的可读性更好。

	Yes：
	
		if foo is not None

	No：
	
		if not foo is None
		
*	当实现有丰富的比较的排序操作时，最好实现所有六个操作符（__eq__，__ne__，__lt__，__le__，__gt__，__ge__）而不是依靠其它代码只能进行一个特定的比较。

	为了减少所涉及的工作量，functools.total_ordering()装饰器提供了一个工具来生成缺失的比较函数。
	
	PEP 207表明，Python假定自反性规则。因此，编译器可以交换y > x和x < y，y >= x和x <= y，也可以交换参数x == y和x != y。sort()和min()操作保证使用< 操作符并且max()功能使用> 操作符。不管怎样，最好实现所有六个操作，这样在其它上下文就不会产生混淆了。

*	使用函数定义def代替lambda赋值给标识符：

	Yes：
	
		def f(x): return 2*x

	No：
	
		f = lambda x: 2*x
	
	第一种形式意味着所得的函数对象的名称是‘f’而不是一般的‘<lambda>’。这在回溯和字符串表示中更有用。
	赋值语句的使用消除了lambda表达式可以提供显示def声明的唯一好处（例如它可以嵌在更大的表达式里面）。

*	异常类继承自Exception，而不是BaseException。直接继承BaseException是保留那些捕捉几乎总是错的异常的。

	设计异常层次结构基于区别，代码可能需要捕获异常，而不是捕获产生异常的位置。旨在以编程方式回答问题“出了什么问题？”，而不是只说“问题产生了”（参见PEP 3151对这节课学习内置异常层次结构的一个例子）
	
	类的命名规则适用于此，只是当异常确实是错误的时候，需要在异常类名添加“Error”后缀。用于非本地的流控制或其他形式的信号的非错误的异常，不需要特殊的后缀。

*	适当使用异常链。在Python3，“raise X from Y”用来表明明确的更换而不失去原来追踪到的信息。

	当故意替换一个内部异常（Python 2中使用“raise X”而Python 3.3+中使用“raise X from None”），确保相关的细节被转移到新的异常中（比如当转换KeyError为AttributeError时保留属性名，或在新的异常消息中嵌入原始异常的文本）。

*	Python2中用 "raise ValueError('message')" 代替 "raise ValueError, 'message'"

	后者不兼容Python3语法。
	
	目前使用的形式意味着当异常的参数很长或包含格式化字符传时，多亏了小括号,不必再使用续行符。
	
*	捕获异常时尽量指明具体异常，而不是空"except:"子句。比如：

		try:
		    import platform_specific_module
		except ImportError:
		    platform_specific_module = None
		    
	空"except:"子句(相当于except Exception)会捕捉SystemExit和KeyboardInterrupt异常，难以用Control-C中断程序，并可掩盖其他问题。如果你想捕捉信号错误之外所有的异常，使用"except Exception"。

	空"except:"子句适用的情况两种情况：

	1. 打印出或记录了traceback，至少让用户将知道已发生错误。 
	2. 代码需要做一些清理工作，并用 raise转发了异常。这样try...finally可以捕捉到它。

*	Python 2.6以后建议用as显示绑定异常名：

	Yes：
	
		try:
		    process_data()
		except Exception as exc:
		    raise DataProcessingFailedError(str(exc))
		    
	这样才能兼容Python3语法并避免歧义。

*	捕捉操作系统错误时，建议使用Python 3.3引入显式异常层次，支持内省errno值。

*	此外所有try/except子句的代码要尽可的少，以免屏蔽其他的错误。

	Yes：
	
		try:
		    value = collection[key]
		except KeyError:
		    return key_not_found(key)
		else:
		    return handle_value(value)

	No：
	
		try:
		    # 太泛了!
		    return handle_value(collection[key])
		except KeyError:
		    # 会捕捉到handle_value()中的KeyError
		    return key_not_found(key)
    
*	本地资源建议使用with语句，以确保即时清理。当然try / finally语句也是可以接受的。

*	上下文管理器在做获取和释放资源之外的事情时，应通过独立的函数或方法。例如：

	Yes:
	
		with conn.begin_transaction():
		    do_stuff_in_transaction(conn)

	No:
	
		with conn:
		    do_stuff_in_transaction(conn)
		    
	后者的例子没有提供任何信息表明enter和__exit__方法做了什么，除了事务结束后关闭连接。 在这种情况下，明确是很重要的。

* 	返回语句保持一致。函数中的所有返回语句都有返回值，或都没有返回值。如果任意一个返回语句有返回值，那么任意没有返回值的返回语句应该明确指出return None，并且一个显式的返回语句应该放在函数结尾（如果可以）。

	Yes:
		
		def foo(x):
    			if x >= 0:
        			return math.sqrt(x)
    			else:
        			return None

		def bar(x):
    			if x < 0:
        			return None
    			return math.sqrt(x)

	No:
    		
    		def foo(x):
    			if x >= 0:
        			return math.sqrt(x)

		def bar(x):
    			if x < 0:
        			return
        		return math.sqrt(x)
		    
*	使用字符串方法而不是string模块。

	python 2.0以后字符串方法总是更快，且Unicode字符串相同的API。

*	使用使用 .startswith()和.endswith()代替字符串切片来检查前缀和后缀。and
startswith()和endswith更简洁，利于减少错误。例如：

	
	Yes:
	
		if foo.startswith('bar'):

	No:
	
		if foo[:3] == 'bar':
		
*	使用isinstance()代替对象类型的比较：

	Yes:
	
		if isinstance(obj, int):

	No:
	
		if type(obj) is type(1):
		
	当检查一个对象是否是字符串时，牢记它也可能是一个unicode字符串！Python 2中，str和unicode有共同的基类, basestring，所以你可以这么做

		if isinstance(obj, basestring):

	注意，Python3中， unicode 和 basestring 不再存在（只有str），并且字节对象不再是string的一种（而是一个integers序列）
	
*	对序列(字符串、列表 、元组), 空序列为false:

	Yes:

		if not seq:
		if seq:

	No:

		if len(seq):
		if not len(seq):

*	不要书写依赖后置空格的字符串。这些后置空格在视觉上无法区分，并且有些编辑器（或最近，reindent.py）将去掉他们。

*	不要用 == 进行布尔比较

	Yes:
	
		if greeting:
		
	No:
	
		if greeting == True		
		if greeting is True: # Worse
		
## 函数注释

为了包含 PEP 484 ，风格指南的函数注释部分进行了更新

*	为了向前兼容,Python 3中的函数注释代码应该更好地使用PEP 484语法。(在前一节中有一些注释格式的建议)。

*	不再鼓励之前在这个PEP中推荐的尝试性的注释风格。

*	然而,除了标准库,现在鼓励使用PEP 484中的尝试性的规则。例如,使用PEP 484风格类型的注释标记一个大型第三方库或应用,检查添加这些注释有多容易,并观察他们的是否会使代码更加易懂。

*	Python标准库应该对使用这些注释采取保守的态度,但允许在新的代码或大型重构中使用这些注释规则。

*	对于代码，如果想要用函数注释的不同用法，推荐使用如下格式的注释:
	
		# type: ignore

	在靠近文件的顶部,这告诉类型检查器忽略所有注释。(更详细的关闭类型检查器报错的方法，可以在 PEP 484 中查找)。

*	像linters，类型检查，是可选的,单独的工具。Python解释器在默认情况下不应该发布任何类型检查的消息，并且应该不改变他们基于注释的行为。

*	不想使用类型检查的用户可以自由地忽略它们。然而,可能用户的第三方库包可能想要在这些包种运行类型检查。为此，PEP 484建议使用存根文件:'.pyi' 文件被类型检查器读取相应的'.py'文件的偏好。存根文件可以通过资源和库一起发布，或在获得库作者的许可时单独发布。

*	对于需要向后兼容的代码,函数注释可以添加到评论格式中。可以查看 PEP 484 中相关的章节。
