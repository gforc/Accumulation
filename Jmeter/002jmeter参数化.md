参数化是自动化测试脚本的一种常用技巧。简单来说，参数化的一般用法就是将脚本中的某些输入使用参数来代替，在脚本运行时指定参数的取值范围和规则；

这样，脚本在运行时就可以根据需要选取不同的参数值作为输入。这种方式通常被称为数据驱动测试（Data Driven Test），参数的取值范围被称为数据池
（Data Pool）。

 

#jmeter中，常用的几种参数化方式：

【测试计划】面板中定义的变量，变量作用域为所有线程；




【配置元件/User Defined Variables】，变量作用域依据所处位置有所不同；


【配置元件/CSV Data Set Config】，是参数化必不可少的组件配置，相较于函数方式的参数读取更加便捷，且易于测试脚本的管控，其变量作用域依据所处位置有所不同；


【前置处理器/后置处理器】中生成的变量，作用域为当前线程组。










#变量引用的语法如下：
${VARIABLE} 
如果测试计划中引用了未定义的变量或者函数，那么JMeter并不会报告/记录错误信息，引用返回的值就是引用自身。例如，假设字符串UNDEF没 有被定义为变量，
那么${UNDEF}返回的值就是${UNDEF}。变量、函数（包括属性）都是大小写敏感的。JMeter 2.3.1及其后续版本会剔除参数名中的空格，例如，
${__Random(1,63, LOTTERY )}中的"LOTTERY "会被"LOTTERY"所代替。


属性不同于变量。变量对线程而言是局部的，所有线程都可以访问属性，就使用__P或者__property函数。JMeter内置了一些基本函数变量，可以直接引用
 JMeter内置函数列表

|函数类型 	|函数名称 |注释|
| ------- | --------|--------------------|
|Information|	threadNum |get thread number|
|Information	 |machineName |get the local machine name|
|Information|	time |return current time in various formats|
|Information| log |log (or display) a message (and return the value)|
|Information| logn| log (or display) a message (empty return value)|
|Input|	StringFromFile| read a line from a file|
|Input|	FileToString| read an entire file|
|Input|	CSVRead| read from CSV delimited file|
|Input|XPath	|Use an XPath expression to read from a file|
|Calculation|	counter|	generate an incrementing number|
|Calculation|	intSum|	add int numbers|
|Calculation|	longSum	|add long numbers|
|Calculation|	Random|	generate a random number|
|Scripting|	BeanShell|	run a BeanShell script|
|Scripting|	javaScript|	process JavaScript (Mozilla Rhino)|
|Scripting|	jexl|	evaluate a Commons Jexl expression|
|Properties|	property|	read a property|
|Properties|	P|	read a property (shorthand method)|
|Properties|	setProperty|	set a JMeter property|
|Variables|	split|	Split a string into variables|
|Variables|	V	|evaluate a variable name|
|Variables|	eval	|evaluate a variable expression|
|Variables|	evalVar|	evaluate an expression stored in a variable|
|String|	regexFunction	|parse previous response using a regular expression|
|String|	char	|generate Unicode char values from a list of numbers|
|String|	unescape|	Process strings containing Java escapes (e.g. \n & \t)|
|String|	unescapeHtml|	Decode HTML-encoded strings|
|String|	escapeHtml	|Encode strings using HTML encoding|


需要注意，目前变量不支持嵌套；例如${Var${N}}不能正常工作。但是在JMeter 2.2及其以后版本中，可以借助函数__V (variable)来达成嵌套变量的目的
（如 ${__V(Var${N})}）。在早期的JMeter版本中可以使 用${__BeanShell(vars.get("Var${N}")}。
