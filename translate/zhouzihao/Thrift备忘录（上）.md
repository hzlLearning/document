# Thrift 备忘录

标签（空格分隔）： Thrift 

---

原作者Diwaker Gupta 
2015-11-08

翻译 ： 周子豪
2016-07-10

针对 `thrift0.6.0`

来自[Thrift官网](http://thrift.apache.org)的介绍:

> Thrift是一种可扩展的跨语言的服务器开发框架。他结合了一个软件栈和一个代码生成引擎来构建服务，以使得在多语言如：C++, Java, Python, PHP, Ruby, Erlang, Perl, Haskell, C#, Cocoa, JavaScript, Node.js, Smalltalk,和 OCaml之间有效和无缝的工作。

Thrift有这丰富的特性。它最严重缺乏的是*优秀的文档*。这个指南意图去填补这个不足。但是请注意这个是一个参考指南-一步一步列子告诉你如何使用Thrift的参考指南。
这部指南的许多结构组织方法借鉴了（优先的）[Google Protocol Buffer Language Guide](http://code.google.com/apis/protocolbuffers/docs/proto.html)我非常感谢这部文档的作者。

这还提供了本片的[PDF文档](http://diwakergupta.github.io/thrift-missing-guide/thrift.pdf)(英文版)

**版权**

英文原版版权  Copyright © 2013 Diwaker Gupta
这篇文章的许可证在[Creative Commons Attribution-NonCommercial 3.0 Unported License.](https://creativecommons.org/licenses/by-nc/3.0/)下。

**贡献**

我欢迎大家对这个参考书进行反馈和贡献。你可以找到这个的[源码](https://github.com/diwakergupta/thrift-missing-guide)在[github](https://github.com/)上。

**致谢**

我感谢Thrift的开发者们，感谢Google Protocol Buffer文档的作者给与我的灵感，感谢Thrift社区的反馈。特别感谢Dave Engberg在Evernote（软件未知！）对本文的帮助。 

**关于作者**

我是一名开源社区的极客和软件架构师。你可以在我的博客[浮动太阳](https://floatingsun.net/)和[这](https://diwakergupta.info/)找到更多关于我的信息。
( *提示：这两个地址均在国内无法访问* )

---

[TOC]

---

##语言参考

### 类型

Thrift的类型系统由`预定义基本类型`、`用户定义结构体`、`容器类型`、`异常类型`和`服务定义器`组成。

#### 基本类型

 - bool：一个布尔型数据（真或假），占一比特
 - byte：有符号比特类型
 - i16：一个16位的有符号整型
 - i32：一个32位的有符号整型
 - i64：一个64位的有符号整型
 - double：一个64位的浮点数
 - binary：一个比特数组
 - string：一个不可预料的码的文本或二进制字符串
 
请注意Thrift不支持无符号的整型，因为在许多Thrift的目标语言的原生类型中没有相应的翻译。

#### 容器类型

Thrift的容器类型是强类型的容器，它对应了许多流行编程语言中容器类型的用法。它们使用了JAVA语言的泛型标注方式。这里有三个容器类型可供使用：

 - list<t1>：一个以t1类型为元素的有序表。可嵌套。
 - set<t1>：一个以t1为不重复元素的无序集合
 - map<t1,t2>：一个以t1元素为键以t2为值的键值对（HashMap）。

#### 结构体和异常

Thrift的结构体从概念上和C语言的结构体类似，是一种组织（封装）有关系类目的方便的方式。结构体在面向对象的语言中被翻译为类。
异常在结构和功能上等同于结构体之外，仅使用关键字`exception`代替关键字`struct`。
他们的不同在于语义，定义RPC服务时，开发人员可能会发布一个远程方法来抛出异常。

#### 服务

服务定义在语义上等同于在面向对象的程序设计中定义一个接口（或一个纯粹的虚拟抽象类）。Thrift编译器生成具有全功能的客户端并在服务端实现这些接口。
有关服务器的详细定义在后一个章节。

### 定义类型(Typedefs)

Thrif支持C/C++的定义类型。

```  
    typedef i32 MyIngter   // [1]
    typedef Tweet ReTweet  // [2]
```    

[1]注意这里没有`;`号
[2]结构体也可以被用作类型定义

### 枚举类型

当你定义一个消息类型时，您可能希望它的一个字段只有一个预定义列表中的一个字段的值。例如：你想要在每个`Tweeet`中增加一个`TweetType`，但是`TweetType`只可能是`TWEET`, `RETWEET`, `DM`, 或 `REPLY`。你可以简单的增加一个枚举类型来定义你消息，一个枚举类型字段只能有一个指定的一组常量的值（如果你尝试提供一个不同的值，解析器将把它像一个未知的字段
）在接下来的例子中我们会增加一个枚举类型叫TweetType列出所有可能的值，并在结构体中添加这个字段：

```
enum TweetType {
    TWEET,       // [1]
    RETWEET = 2, // [2]
    DM = 0xa,    // [3]
    REPLY
}                // [4]

struct Tweet {
    1: required i32 userId;
    2: required string userName;
    3: required string text;
    4: optional Location loc;
    5: optional TweetType tweetType = TweetType.TWEET // [5]
    16: optional string language = "english"
}
```

[1]枚举类型是c语言风格。将在没指定的情况下默认0开始。
[2]你当然也可以给他提供一个特定的整型值。
[3]16进制也是被允许的。
[4]注意结尾没有`;`号
[5]在指定默认值时使用常量的完全限定名称。

值得注意的是：不想协议栈，Thrift还**不支持**嵌套的枚举类型（或在里面嵌套结构体）
枚举常量**必须**在32位正整数的范围内。

### 注释

Thrift支持shell、C风格的多行注释和java、C++风格的单行注释。

```
# This is a valid comment.

/*
 * This is a multi-line comment.
 * Just like in C.
 */

// C++/Java style single-line comments work just as well.
```

### 名称空间

Thrift的命名空间类似于C++的命名空间或java的包，它提供了一种方便的组织（或分离）代码的方法。命名空间也用于防止类型定义之间的名称冲突。
因为每种语言有他自己的包依赖习惯。（如python的模块modules）Thrift允许你在每个语言的基础上自定义命名空间的行为：

```
namespace cpp com.example.project  // [1]
namespace java com.example.project // [2]
```

[1]翻译为命名空间 ` com { namespace example { namespace project { `
[2]翻译为包 `com.example.project`

###包含（Include）

