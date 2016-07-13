# **2. 代码生成(Generated Code)**

标签（空格分隔）： Thrift

---
上一篇，[基于CI框架的网站][1]，欢迎点击！
欢迎访问[我的Github][2]，好东西都在里面哟！
[本文原文地址][3]

---

该部分内容包含不同环境下用thrift生成不同语言代码的文档。我们首先介绍全线使用的共同概念，这些概念关系到代码生成的结构，希望它能帮助你如何高效的去使用thrift。
##**2.1. 概念**
下面是thrift网络堆栈示意图:

<img src="http://oa5lp7rpl.bkt.clouddn.com/figure1.png">

### **传输**
传输层提供了一个简单的读写抽象给网络层，这就使得thrift的其他部分和系统的底层分离。（例如：序列化/反序列化）
下面是一些由传输接口公开的方法：
> * open
> * close
> * read
> * write
> * flush

除了上面的`传输`接口，thrift也适用于`ServerTransport`接口来接收或者创建原始传输对象。正如其名称暗示的，`ServerTransport`主要用于服务器端创建新的传输对象给下面的接口。

> * open
> * listen
> * accept
> * close

下面是一些可用于大多数thrift支持语言的传输方式：
文件：从硬盘上进行文件读写
http：as the same suggest

### **协议(`Processor`)**
`Processor`抽象地定义了一种机制，将内存中的数据结构映射到一个线制格式.换言之,一个协议指定的数据类型如何对他们自己使用底层传输到编码/解码.因此，协议实现支配编码方案，并负责序列化。在这个意义上的协议的一些例子包括JSON，XML，纯文本，严谨的二进制等。

下面是`Processor`的接口：
```
writeMessageBegin(name, type, seq)
writeMessageEnd()
writeStructBegin(name)
writeStructEnd()
writeFieldBegin(name, type, id)
writeFieldEnd()
writeFieldStop()
writeMapBegin(ktype, vtype, size)
writeMapEnd()
writeListBegin(etype, size)
writeListEnd()
writeSetBegin(etype, size)
writeSetEnd()
writeBool(bool)
writeByte(byte)
writeI16(i16)
```
```
writeI32(i32)
writeI64(i64)
writeDouble(double)
writeString(string)
name, type, seq = readMessageBegin()
readMessageEnd()
name = readStructBegin()
readStructEnd()
name, type, id = readFieldBegin()
readFieldEnd()
k, v, size = readMapBegin()
readMapEnd()
etype, size = readListBegin()
readListEnd()
etype, size = readSetBegin()
readSetEnd()
bool = readBool()
byte = readByte()
i16 = readI16()
i32 = readI32()
i64 = readI64()
double = readDouble()
string = readString()
```
thrift协议被设计为面向流，它没有必要明确任何帧。例如，在我们开始序列之前，它是没有必要知道字符串或列表中的项目的数目的长度，下面是一些可用于大多数thrift支持语言的协议：

> * binary: 相当简单的二进制编码,一个字段的长度和类型被编码为字节随后字段的实际值。

> * compact: 在 [THRIFT-110][4] 被提到过

> * json:


###**处理器**
处理器封装了输入和输出的能力，这种输入输出流在`Processor`对象上面就能体现的出来，`Processor`接口是相当简单的：
```
interface TProcessor {
    bool process(TProtocol in, TProtocol out) throws TException
}
```
特定服务的处理器实现是由编译器生成的。处理器基本上从线路（使用输入协议）和代表处理的处理程序，通过线路的响应（使用输出协议）读取（由用户实现）和写入数据。

###**Server**
一个服务器把下面所有功能都实现了：
> * 创建了传输层
> * 给传输层封装了输入输出协议
> * 基于上面的协议创建了协议层
> * 等待传入的连接把他们送给处理器

接下来我们讨论特定语言生成的代码。除非另有说明，以下章节将遵循下面的thrift规格：

**IDL 例子**
```
namespace cpp thrift.example
namespace java thrift.example

enum TweetType {
    TWEET,
    RETWEET = 2,
    DM = 0xa,
    REPLY
}

struct Location {
    1: required double latitude;
    2: required double longitude;
}

struct Tweet {
    1: required i32 userId;
    2: required string userName;
    3: required string text;
    4: optional Location loc;
    5: optional TweetType tweetType = TweetType.TWEET;
    16: optional string language = "english";
}

typedef list<Tweet> TweetList

struct TweetSearchResult {
    1: TweetList tweets;
}

exception TwitterUnavailable {
    1: string message;
}

const i32 MAX_RESULTS = 100;

service Twitter {
    void ping(),
    bool postTweet(1:Tweet tweet) throws (1:TwitterUnavailable unavailable),
    TweetSearchResult searchTweets(1:string query);
    oneway void zip()
}
```
**嵌套的结构是如何初始化的**

  [1]: https://www.zybuluo.com/klci/note/430232
  [2]: https://github.com/ab233
  [3]: https://www.zybuluo.com/klci/note/431393
  [4]: https://issues.apache.org/jira/browse/THRIFT-110