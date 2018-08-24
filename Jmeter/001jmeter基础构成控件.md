# 两种基础构成控件
 
## Test Plan (测试计划)：
用来描述一个测试，包含与本次测试所有相关的功能。也就说测试的所有内容是于基于一个计划的。

## Threads （Users）线程 用户
1) setup thread group  
　一种特殊类型的ThreadGroup的，可用于执行预测试操作。这些线程的行为完全像一个正常的线程组元件。不同的是，这些类型的线程执行测试前进行定期线程组的执行。
2) teardown thread group.  
　一种特殊类型的ThreadGroup的，可用于执行测试后动作。这些线程的行为完全像一个正常的线程组元件。不同的是，这些类型的线程执行测试结束后执行定期的线程组。
3) thread group(线程组).
   这个就是我们通常添加运行的线程。通俗的讲一个线程组，可以看做一个虚拟用户组，线程组中的每个线程都可以理解为一个虚拟用户。线程组中包含的线程数量在测试执行过程中是不会发生改变的。





 
## jmeter中的八种元件(componment)，我们所有的case操作步骤基本都是由这八种元件组成。
 
### 取样器/采样器（Sampler）
       取样器（Sampler）是性能测试中向服务器发送请求，记录响应信息，记录响应时间的最小单元，JMeter 原生支持多种不同的sampler ，如 HTTP Request Sampler 、 FTP  Request Sample 、TCP  Request Sample 、JDBC Request Sampler 等，每一种不同类型的 sampler 可以根据设置的参数向服务器发出不同类型的请求。

### 逻辑控制器（Logic Controller）
　　逻辑控制器，包括两类元件，一类是用于控制test plan 中 sampler 节点发送请求的逻辑顺序的控制器，常用的有 如果（If）控制器 、switch Controller 、Runtime Controller、循环控制器等。另一类是用来组织可控制 sampler 节点的，如 事务控制器、吞吐量控制器。

### 配置元件（Config Element）
　　配置元件（config element）用于提供对静态数据配置的支持。CSV Data Set config 可以将本地数据文件形成数据池（Data Pool），而对应于HTTP Request Sampler和 TCP Request Sampler等类型的配制无件则可以修改Sampler的默认数据。（例如，HTTP Cookie Manager 可以用于对 HTTP Request Sampler 的cookie 进行管理）
 
### 定时器（Timer）
 　　定时器（Timer）用于操作之间设置等待时间，等待时间是性能测试中常用的控制客户端QPS的手端。类似于LoadRunner里面的“思考时间”。JMeter 定义了Bean Shell Timer、Constant Throughput Timer、固定定时器等不同类型的Timer。
 
### 前置处理器（Per Processors）
　　用于在实际的请求发出之前对即将发出的请求进行特殊处理。例如，HTTP URL重写修复符则可以实现URL重写，当RUL中有sessionID 一类的session信息时，可以通过该处理器填充发出请求的实际的sessionID 。
 
### 后置处理器（Post Processors）
　　用于对Sampler 发出请求后得到的服务器响应进行处理。一般用来提取响应中的特定数据（类似LoadRunner测试工具中的关联概念）。例如，XPath  Extractor 则可以用于提取响应数据中通过给定XPath 值获得的数据。
 
### 断言（Assertions）
　　 断言用于检查测试中得到的相应数据等是否符合预期，断言一般用来设置检查点，用以保证性能测试过程中的数据交互是否与预期一致。
  
### 监听器（Listener）
 　　这个监听器可不是用来监听系统资源的元件。它是用来对测试结果数据进行处理和可视化展示的一系列元件。 图行结果、查看结果树、聚合报告。都是我们经常用到的元件。




## 元件的作用域
 
在jmeter中，元件的作用域是靠测试计划的的树型结构中元件的父子关系来确定的，作用域的原则是：
- 取样器（sampler）元件不和其它元件相互作用，因此不存在作用域的问题。
- 逻辑控制器（Logic Controller）元件只对其子节点中的取样器 和 逻辑控制器作用。
- 除取样器 和 逻辑控制器 元件外，其他6类元件，如果是某个sampler的子节点，则该元件公对其父子节点起作用。
- 除取样器 和 逻辑控制器 元件外的其他6类元件，如果其父节点不是sampler ，则其作用域是该元件父节点下的其他所有后代节点（包括子节点，子节点的子节点等）。





## 元件的执行顺序
 
了解了元件有作用域之后，来看看元件的执行顺序，元件执行顺序的规则很简单，在同一作用域名范围内，测试计划中的元件按照如下顺序执行。
（1）配置元件（config elements ）
（2）前置处理程序（Per-processors）
（3）定时器（timers ）
（4）取样器（Sampler）
（5）后置处理程序（Post-processors） （除非Sampler 得到的返回结果为空）。
（6）断言（Assertions）（除非Sampler 得到的返回结果为空）。
（7）监听器（Listeners）（除非Sampler 得到的返回结果为空）。
