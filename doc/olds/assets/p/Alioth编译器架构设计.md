# Alioth编译器架构设计

> 作者 王雨泽 2019/01/04  
> 本文档描述Alioth编译器的总体架构设计  
> 对应语言版本 Alioth-0.8  
> 对应编译器版本 aliothc-0.8

- [Alioth编译器架构设计](#alioth%e7%bc%96%e8%af%91%e5%99%a8%e6%9e%b6%e6%9e%84%e8%ae%be%e8%ae%a1)
  - [1. 职能](#1-%e8%81%8c%e8%83%bd)
    - [1.1. 概述](#11-%e6%a6%82%e8%bf%b0)
    - [1.2. 编译功能](#12-%e7%bc%96%e8%af%91%e5%8a%9f%e8%83%bd)
    - [1.3. 项目拓扑功能](#13-%e9%a1%b9%e7%9b%ae%e6%8b%93%e6%89%91%e5%8a%9f%e8%83%bd)
  - [2. 接口设计](#2-%e6%8e%a5%e5%8f%a3%e8%ae%be%e8%ae%a1)
    - [2.1. 命令行参数](#21-%e5%91%bd%e4%bb%a4%e8%a1%8c%e5%8f%82%e6%95%b0)
      - [2.1.1. 常规使用范例](#211-%e5%b8%b8%e8%a7%84%e4%bd%bf%e7%94%a8%e8%8c%83%e4%be%8b)
      - [2.1.2. 可用的参数](#212-%e5%8f%af%e7%94%a8%e7%9a%84%e5%8f%82%e6%95%b0)
    - [2.2. 交互式接口](#22-%e4%ba%a4%e4%ba%92%e5%bc%8f%e6%8e%a5%e5%8f%a3)
      - [2.2.1. 指令框架](#221-%e6%8c%87%e4%bb%a4%e6%a1%86%e6%9e%b6)
      - [2.2.2. 指令集](#222-%e6%8c%87%e4%bb%a4%e9%9b%86)
  - [3. 模块设计](#3-%e6%a8%a1%e5%9d%97%e8%ae%be%e8%ae%a1)
    - [3.1. 交互模块](#31-%e4%ba%a4%e4%ba%92%e6%a8%a1%e5%9d%97)
      - [3.1.1. 入口函数](#311-%e5%85%a5%e5%8f%a3%e5%87%bd%e6%95%b0)
      - [3.1.2. 交流模块](#312-%e4%ba%a4%e6%b5%81%e6%a8%a1%e5%9d%97)
      - [3.1.2. 文档引擎](#312-%e6%96%87%e6%a1%a3%e5%bc%95%e6%93%8e)
      - [3.1.3. 日志引擎](#313-%e6%97%a5%e5%bf%97%e5%bc%95%e6%93%8e)
        - [3.1.3.1 可穿梭语法树的日志仓库](#3131-%e5%8f%af%e7%a9%bf%e6%a2%ad%e8%af%ad%e6%b3%95%e6%a0%91%e7%9a%84%e6%97%a5%e5%bf%97%e4%bb%93%e5%ba%93)
    - [3.2. 基础设施](#32-%e5%9f%ba%e7%a1%80%e8%ae%be%e6%96%bd)
      - [3.2.1. 容器](#321-%e5%ae%b9%e5%99%a8)
      - [3.2.2. 代理](#322-%e4%bb%a3%e7%90%86)
      - [3.2.3. Json解析](#323-json%e8%a7%a3%e6%9e%90)
    - [3.3. 词法分析模块](#33-%e8%af%8d%e6%b3%95%e5%88%86%e6%9e%90%e6%a8%a1%e5%9d%97)
    - [3.4. 语法分析模块](#34-%e8%af%ad%e6%b3%95%e5%88%86%e6%9e%90%e6%a8%a1%e5%9d%97)
      - [3.4.1. 语法结构类](#341-%e8%af%ad%e6%b3%95%e7%bb%93%e6%9e%84%e7%b1%bb)
      - [3.4.2. 定义与实现](#342-%e5%ae%9a%e4%b9%89%e4%b8%8e%e5%ae%9e%e7%8e%b0)
      - [3.4.3. 类型系统](#343-%e7%b1%bb%e5%9e%8b%e7%b3%bb%e7%bb%9f)
      - [3.4.4. 名称用例](#344-%e5%90%8d%e7%a7%b0%e7%94%a8%e4%be%8b)
      - [3.4.5. 语法分析算法基础设施](#345-%e8%af%ad%e6%b3%95%e5%88%86%e6%9e%90%e7%ae%97%e6%b3%95%e5%9f%ba%e7%a1%80%e8%ae%be%e6%96%bd)
    - [3.5. 语义分析模块](#35-%e8%af%ad%e4%b9%89%e5%88%86%e6%9e%90%e6%a8%a1%e5%9d%97)
      - [3.5.1. 模块描述符](#351-%e6%a8%a1%e5%9d%97%e6%8f%8f%e8%bf%b0%e7%ac%a6)
    - [3.6. 顶层驱动模块](#36-%e9%a1%b6%e5%b1%82%e9%a9%b1%e5%8a%a8%e6%a8%a1%e5%9d%97)
    - [3.7. 目标代码生成模块](#37-%e7%9b%ae%e6%a0%87%e4%bb%a3%e7%a0%81%e7%94%9f%e6%88%90%e6%a8%a1%e5%9d%97)
  - [4. 类关系图](#4-%e7%b1%bb%e5%85%b3%e7%b3%bb%e5%9b%be)
  - [5.](#5)

## 1. 职能

### 1.1. 概述

`aliothc`是基于`libalioth`开发的用于处理以**Alioth**编程语言为中心的软件开发项目的工具.

其主要功能包括编译功能和项目拓扑功能.

### 1.2. 编译功能

`aliothc`可以从命令行接受参数,从**空间**读取配置文档,将项目中所包含的Alioth源文档编译为可执行文件或静态,动态链接库.

编译前,`aliothc`会对指定空间中所有文件进行分析,缓冲有关**模块**间依赖关系的信息,确保每次项目构建时,能以最少的时间迅速确定编译目标模块的先决条件,闭包模块依赖关系.

在这过程中,后缀名为`.alioth`的文件被视为**源文档**,检测过程中的语法错误或依赖性错误会被报告至**标准输出流**.而后缀名为其他的文件,若包含合法的**模块签名**则被视为**源文档**参与编译,否则会被忽略.

编译时,`aliothc`能检查所有源代码的语法错误,确认无误后建立语法树.再检查所有语句的语义错误,确认无误后编译为目标产物.

`aliothc`也可以将没有语法错误的语法树结构串行化为二进制数据文件,当源文档长期不被编辑时,便可以大幅度节省语法分析带来的开销,直接读取语法树进行后续步骤.

由于**Alioth**语言的设计同时`aliothc`使用`GNU/ld`链接编辑器链接编译产物,只要符号处理得当,`aliothc`可以将由其他语言编译产生的编译产物与`aliothc`的编译产物链接成为目标产物.

### 1.3. 项目拓扑功能

启动时,`aliothc`需要项目的物理部署结构满足特定要求,从而创建抽象**空间**作为`aliothc`的活动范围.

若项目的物理部署结构不满足特定要求,`aliothc`可以根据命令行参数或配置文件,甚至交互式指令跨越路径,甚至跨越文件系统,甚至跨越网络链接将几个流数据源抽象成为**空间**.

启动时,给出特定的参数,`aliothc`会进入交互模式,通过`Json`与标准输入输出流通信.在通信的过程中也可能打开或关闭新的流传输数据.

这样的特性使得`aliothc`可以仅凭一两行脚本,或任何一种可以启动进程的编程语言封装后,成为一个分布式异构环境下的编译器.

## 2. 接口设计

### 2.1. 命令行参数

#### 2.1.1. 常规使用范例

~~~sh
aliothc Hello # Hello 为模块名,aliothc会试图将Hello模块及其依赖编译并链接为一个可执行文件

aliothc -s Hello Bar Foo # 将Hello,Bar,Foo三个模块分别编译为静态链接库

aliothc -d Hello Bar Foo # 将Hello,Bar,Foo三个模块分别编译为动态链接库

aliothc -- # 进入交互模式与标准输入输出流沟通

aliothc -- host 31262 # 进入Host模式监听所有网卡的31262端口向外提供编译算力和资源访问服务
~~~

#### 2.1.2. 可用的参数

|语法|语义|示例|
|---|---|---|
|`-s`|将目标模块编译为静态链接库,与`-d`等参数互斥|
|`-d`|将目标模块编译为动态链接库,与`-s`等参数互斥|
|`-g`|生成调试信息|
|`--conf <filename>`|指定配置文件路径<br/>此参数默认可省<br/>默认编译器会从工作空间和根空间读取配置文件|`--conf https://host:3000/project/common-conf.json`|
|`--work <path>`|指定工作空间路径<br/>默认情况下编译器将`./`视为当前工作空间|`--work ./client/pc/alioth/`|
|`--root <path>`|指定根空间的路径<br/>默认情况下编译器将`/alioth/`视为根空间|`--root alioth://ezr@host:31262/root`|
|`--arc <path>`|配置工作空间下Arc子空间的路径<br/>默认情况下编译器将`./arc/`作为目标||
|`--bin <path>`|配置工作空间下Bin子空间的路径<br/>默认情况下编译器将`./bin/`作为目标||
|`--doc <path>`|配置工作空间下Doc子空间的路径<br/>默认情况下编译器将`./doc/`作为目标||
|`--lib <path>`|配置工作空间下Lib子空间的路径<br/>默认情况下编译器将`./lib/`作为目标||
|`--inc <path>`|配置工作空间下Inc子空间的路径<br/>默认情况下编译器将`./inc/`作为目标||
|`--obj <path>`|配置工作空间下Obj子空间的路径<br/>默认情况下编译器将`./obj/`作为目标||
|`--src <path>`|配置工作空间下Src子空间的路径<br/>默认情况下编译器将`./src/`作为目标||
|`-- [host [<port>]]`|进入交互模式,与标准输入输出流通信<br/>带Host参数进入**宿主模式**,向外提供服务|`-- host 31262`|

### 2.2. 交互式接口

编译器必须通过`SSH`身份校验才能通过`alioth`协议登录一个`alioth宿主`所以指令包中不包含任何身份验证所需的信息.

#### 2.2.1. 指令框架

~~~json
{
    /**
     * 指令码或指令字符串
     */
    "cmd" : 0,

    /**
     * 会话口令
     * 一条指令能打开一次会话,在一次会话的过程中口令不变
     * 口令的作用是使得多条指令可以穿插执行
     * 链接双方分别使用正负数指定指令口令,以确保不可能出现冲突
     */
    "token" : 0,

    /**
     * 指令参数
     * 根据指令的不同,需要携带不同的参数
     */
    ...
}

{
    /**
     * 回复码可以简略描述指令执行的情况
     */
    "reply" : 0,

    /**
     * 回复口令要和回复的指令的口令保持一致
     * alioth使用TCP通信,不考虑掉包,重复,或先发后至的情况
     * 所以token没必要变化
     */
     "token" : 0,

     /**
      * 根据指令的不同,回复的内容也可能不同
      * 甚至有可能不存在内容
      * 但是不论回复内容数据量多庞大,都不会分包发送
      * 因为TCP对此情况已经有所处理
      */
      ...
}
~~~

#### 2.2.2. 指令集

[TODO]

## 3. 模块设计

### 3.1. 交互模块

交互模块用于从外界接受**用户**的指令,控制编译器的执行.

交互模块也用于与外界建立数据通路,隐藏项目物理部署细节,向其他模块提供抽象的**空间**环境.

交互模块也用于隐藏人类可读性细节简化编译器内部表示型信息的产生和存储.

#### 3.1.1. 入口函数

`main`函数作为编译器的入口函数,从命令行读取参数,初始化编译器的各个模块,之后将控制权交给**管理器**.

#### 3.1.2. 交流模块

`Cengine`作为交流模块,可以绑定端口建立`Alioth宿主`,也可以与`Alioth宿主`通信,向文档引擎提供数据源.

交流模块同时还负责身份认证流程.

`Cengine`用于管理所有基于`ALIOTH`通信协议的网络连接,所有基于`alioth://`前缀的路径访问请求都应该交由`Cengine`模块处理.

#### 3.1.2. 文档引擎

`Dengine`作为文档引擎,通过配置,建立抽象空间与文件系统,数据流,网络连接之间的映射关系,向其他模块提供面向空间的抽象文件系统.

文档引擎应当能处理除了自然文件系统访问外的协议请求,包括`HTTP`请求,`HTTPS`请求,`FTP`请求,`SFTP`请求,`ALIOTH`请求.

以`Dengine`为代表,所有模块的对文件的处理都面向流.`Dengine`会根据请求所使用的协议不同,将面向空间的请求映射为其他协议的请求,转发给其他模块,其他模块请求成功后,都将文件的输入或输出流返回给最初的请求者,减小数据缓冲的压力.

#### 3.1.3. 日志引擎

`Lengine`作为日志引擎,用于将特定的模式作用于`日志项`逻辑结构,产生`日志项`数据结构与`日志`文本之间的映射关系.隐藏人类可读细节,简化其他模块产生日志的复杂度.

日志引擎还可以根据情况将日志文本封装成规范化的`Json`对象,携带足够的相关信息,供IDE提供交互式的基于渲染的报错提示功能.

##### 3.1.3.1 可穿梭语法树的日志仓库

日志引擎中包含了很多子类,用于对各种级别的日志数据进行抽象,其中最特殊的是`Logr`,表示一个日志仓库.

`Logr`不是语法结构,但它继承自`thing`,这个设计使得`Logr`可以被用于承载语法结构的代理`agent`承载并在语法树中穿梭.

这样设计的原因是,很多语法结构会在被使用时,产生新的语法结构或发生改变,而这时产生的语义错误如果不被报告,就再也不会被检查一次了,但是`find`或`request`方法都不应该报告日志,这不是它们的职责,所以最终的解决方案就是,当对象请求某个语法结构是,发生了错误,`find`或`request`方法返回失败,但结果集可能不是空的,而是包含了若干个日志仓库,其中又存储了一定数量的报错信息,发起请求的对象可以根据自己当前的职责选择是否将错误报告到日志引擎.

### 3.2. 基础设施

#### 3.2.1. 容器

`chainz`类使用指针表策略存储对象序列,具有以下特性.

* 多个对象不必存储于连续的空间中
* 容器的拼接,移动,对象在容器中和容器间的移动,实际上都不涉及对象自身的拷贝或移动
* 对象可以动态的随机增删改查,但查询时间复杂度保持`O(1)`不变
* 对象可以直接在容器内构造,也可以被提出容器后再析构,所以即使对象的构造函数或析构函数私有,也依然可以存入chainz
* chainz提供了迭代器类用于支持`range-based-for`语法,也可用于提供平坦的搜索过程

多数语法结构不允许被拷贝,因为它们在语法树中上下关联关系通常复杂而不对称.

chainz为语法结构提供了比较可行的存储策略.

#### 3.2.2. 代理

`agent`模板类和`thing`类共同为其他模块提供了基于引用计数的内存管理策略.

所有可能需要`agent`管理的对象,必须继承自`thing`类.

`agent`对象类似于`C++`中的**智能指针**,差别在于,引用计数随着对象与`thing`类的继承关系,被存储在对象内部.这点保证了随时以任何形式产生一个代理指向某个对象都是安全的,只要对象不在栈中.

另外由于所有`agent`管理的对象都继承自`thing`,所以实际上`agent`内部只存储了`thing`指针,这使得用于管理不同类型的对象的`agent`比如`agent<methop>`和`agent<classd>`之间的本质上是没有差别的,它们之间可以进行类型安全的比较,赋值,类型转换.

代理对自身有主动的运行时类型检查,一旦类型不符,指针会被归零,确保任何访问都是类型安全的,其他模块非常需要这一特性来简化代理所指向的对象的类型判断.

#### 3.2.3. Json解析

`Jsonz`类提供了非常方便的`Json`脚本的解析,并且`Jsonz`对象自身拥有数据类型模糊能力,在其能承受的范围内,通过赋值语句就能改变其中存储的数据的数据类型.

`Jsonz`还提供了将`Json`对象串行成为二进制版本的功能,用于处理一些需要表示却不需要考虑可读性的数据,比如语法树结构缓冲数据.

### 3.3. 词法分析模块

`token`类是词法分析模块唯一的类.

`token`类本身用于表述一个词法记号,或语法记号,携带了记号的起止行列,记号的原书写形式等信息.

`token`类还提供了一个唯一的词法分析接口,用于提供面向输入流的词法分析功能.

`token`提供的词法分析函数中还集成了一个小规模的语法分析器,其提供的功能是当有需要,词法分析器可以选择只分析完整的模块签名,后续内容不再继续分析.此功能在模块依赖关系分析阶段使用,因为这个阶段只需要模块签名信息,这个设计避免了分析大量无用信息.

### 3.4. 语法分析模块

语法分析模块在类定义上就不存在清晰界限了.

每种语法结构都对应一种类定义,将书写形式所携带的信息存储在特定的数据结构中.

每种语法结构类都存在`Create`方法用于实现面向`token流`的语法分析算法.

#### 3.4.1. 语法结构类

`stmt`类继承自`thing`类,作为语法结构的公共基类存在.

`stmt`类存储了所有语法结构都必须存储的基本信息,并规定了所有语法结构必须拥有的行为.

`stmt`提供了`find`和`request`方法分别用于通过源代码中书写的名称在语法树中自顶向下和自底向上搜索和请求语法结构,每个不同的语法结构都应该重载这两个函数.

`find`和`request`的设计将`符号表`的功能融合在语法树中,因为Alioth中存在复杂的透明定义,名称路由和别名定义规则,平坦的符号表不一定能完全胜任语法结构查找工作.

#### 3.4.2. 定义与实现

Alioth语言明确区分了定义和实现的差别,并规定定义之内只能定义,确保源代码的整洁.

`definition`和`implement`类都继承自`stmt`类,分别作为**定义语句**和**实现语句**的公共基类存在.

`classd`,`enumd`,`methop`和`operatord`是四种语法结构类,继承自`definition`表达四种定义语法结构.

`method`,`operat`,`expr`等类继承`implement`类,表述实现语句结构.

#### 3.4.3. 类型系统

Alioth使用元素表示操作内存中对象的媒介,元素的表征是元素的名称和元素原型.

元素原型由元素类型和数据类型构成.

元素类型取自`var ptr ref val`四者之一,描述元素的语义和行为特征.

数据类型根据其书写形式不同,可能与某个类定义或基础数据类型相关联,描述元素所绑定的对象的物理特征和行为特征.

数据类型`dtype`和元素原型`eproto`分别作为两种独特的语法结构存在,它们不属于定义也不属于实现,在语法树中可以作为枝干存在.

#### 3.4.4. 名称用例

对某个名称的使用可能很复杂比如

~~~cpp
alioth::tuple<string<int> network::stream<char>>::typeID
~~~

其中包含了数据类型,模板参数,名称,作用域符等一系列内容.

这样的名称不是一个命名,而是对一个命名的使用,而这个被使用的命名是否存在,在所有源文档语法分析都完全结束之前无法检查.

名称用例也有可能被任何形式使用,比如元素的数据类型名,类定义的基类名称,而名称用例却不一定总是指向被期望的内容,因为名称用例可以指向模板参数,可以指向别名,参数,或另一个类定义,甚至是一个模块.

基于名称的搜索,又由于各种透明机制和别名机制而变得复杂,当名称用例携带了一个模板参数列表,甚至有可能在搜索过程中创造一个新的类定义,如果模板参数列表存在语义问题,又会在这时产生报错信息.

所以名称用例不得不被归类为一种语法结构,在语义分析阶段再检查它的语法.

#### 3.4.5. 语法分析算法基础设施

所有语法结构的语法分析都基于面向状态的最左归约文法分析算法,`state`和`smachine`两个类为这个算法提供基础设施.

`state`表述一个状态,描述了状态所携带的记号数量.

`smachine`表述一个拥有四种动作的状态机,通过栈存储状态,状态机是与记号输入流绑定的,状态机的每个动作都会改变记号输入流的读取指针的位置,甚至改变记号序列的内容.

在每个状态,是否归约某个单词`vn`由使用`smachine`的函数决定,由状态的变化,将一系列不同状态下归约单词的规则串在一起,就形成了对某个语法结构的分析算法.

`smachine`支持如下四种动作

* 移进: 将s状态入栈,将n个记号计入s状态,将输入迭代器前进n个单位.  
  移进动作不会销毁记号,只是记录每个状态对应了多少个记号

* 归约: 将栈顶n个状态中记录的记号,连同或忽略迭代器当前指向的记号归纳成为一个非终结符N,适当调整迭代器位置,使其指向刚刚归约的非终结符  
  归约动作会改变记号输入流的内容,归约的粒度是状态,所以只要适当安排装填,就能使语法分析算法很轻松的应对比较复杂的语法设计.

* 移出: 以状态为单位回滚输入进程,这个功能可以用于设计带回滚的语法分析算法,用于匹配后置语义的文法.

* 停滞: 将n个记号记录在当前状态,前进输入迭代器,此功能与面向状态的归约功能搭配,可以很轻松的应对可选的或循环的文法分支

### 3.5. 语义分析模块

语义分析模块没有专门的类来实现功能,语义分析功能分布在每个语法结构类的`verifySemnatics`方法中,也在一些其他特有的方法中.

语义分析在完整的语法分析之后进行,语义分析的过程中会涉及到基于名称用例的语法结构的搜索和请求,这些动作可能引起新的语法结构的产生.

#### 3.5.1. 模块描述符

模块描述符用于将分不在不同源文档,但描述同一个模块的语法树整合到一个对象中,统一进行管理.

模块描述符同时保留了每个单独的语法树的树根,也即`module`对象,并确保这些`module`对象都以模块描述符为语法树根.

模块描述符会将所有模块中的语法结构统一取出来,存入模块描述符的存储空间,因此,当语法树中自上而下的搜索时,通过模块描述符能准确的找到语法结构.

但是每个语法结构并没有改变其所知的所在作用域,也就是说,从这些语法结构出发,在语法树上自下而上的搜索语法结构,会先到达`module`对象,然后才到达模块描述符.

这样设计的目的是使得从任何语法结构都能得知语法结构被书写在哪个源文档中,因为`module`记录着自己是从哪个源文档分析出来的,但是模块描述符却没有这样的记录,因为模块描述符中的语法结构来自多个源文档.

模块描述符在语义分析阶段起到两个作用,它是每个模块的语义分析的起点,语义分析流程从这里开始检查.当用户书写了跨越模块的名称用例时,请求会经过层层转发,被模块描述符接受,模块描述符会将请求转发至正确的模块,继续进行搜索.

### 3.6. 顶层驱动模块

`Manager`类拥有顶层驱动的职责,由它安排依赖检查,词法分析,语法分析,语义分析的执行流程.

`Manager`管理着其他所有引擎的实例,协调它们之间的工作,提供一个抽象的Alioth工作环境.

`Manager`还管理着`event`信箱,`event`描述了一个动态发生的事件,被向上报告,最终由`Manager`接收,并影响接下来的行动安排.

比如,模板类的用例由携带模板参数列表的名称用例搜索时产生.

当一个模板类定义接到了指向自己并携带了模板参数列表的名称用例的搜索请求时,模板类定义会试图产生一个模板用例,是一个被实例化的模板类,一个确切的类定义,并将其作为结果回送.

而这个过程很有可能发生在这个模板类所在的模块已经被语义分析过后,也就是说如果置之不理,新产生的类定义就不会经历语义分析流程,一定会遗留语义错误,造成不能预测的结果.

解决方案就是,当模板类成功的产生了一个用例时,它会产生一个`event`描述这一事件的发生,并沿着语法树向上报告这一`event`,当模块描述符最后接收到这个报告,会转交给`Manager`.

`Manager`在所有模块都进行了语义分析之后,会检查`event`信箱,若其中包含了描述`新语法结构产生`事件的`event`,Manager会专门为它们安排额外的语义分析周期.

### 3.7. 目标代码生成模块

[TODO]

## 4. 类关系图

![](../i/Alioth编译器架构.png)

## 5. 