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


  [1]: https://www.zybuluo.com/klci/note/430232
  [2]: https://github.com/ab233
  [3]: https://www.zybuluo.com/klci/note/431393
  [4]: http://oa5lp7rpl.bkt.clouddn.com/figure1.png