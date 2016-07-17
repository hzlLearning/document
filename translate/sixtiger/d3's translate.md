![D3](https://camo.githubusercontent.com/722a5cc12c7d40231ebeb8ca6facdc8547e2abf7/68747470733a2f2f64336a732e6f72672f6c6f676f2e737667)
#**DATA-DRIVEN DOCUMENTS**   

---
翻译：SixTiger   
时间：2016年7月12日

---
#D3概览
---
**D3**是一个基于数据来操纵文档对象的JavaScript库。如果你在开发中使用了HTML、SVG以及CSS，**D3**可以   让你的数据变得生动形象。基于web标准，D3的重点在于赋予你完善的现代浏览器功能而无需绑定到特定的框架，结合功能强大的可视化组件还有一个 ***数据驱动*** 的方法来实现DOM的操作。   ***[查看更多的例子](https://github.com/d3/d3/wiki/Gallery)***

**最新的下载地址在这里(4.1.1)：**                                
   
* [d3.zip](https://github.com/d3/d3/releases/download/v4.1.1/d3.zip )
   
 **如果想直接连接到最新发行的版本，请复制以下片段：**  
 >      <script src="https://d3js.org/d3.v4.min.js"></script>   
 
 
    
 [完整的资源以及实例](https://github.com/d3/d3)可以在Github上 [下载](https://github.com/d3/d3/zipball/master)。如果你想为我们的不断成长提供支持，请购买我们的****[官方贴纸](https://www.stickermule.com/user/1070696243/stickers)****！。

---
##介绍
---
**D3**允许你把任意类型的数据绑定到文档对象模型中(DOM)，然后应用数据驱动转换到文档上面。比如说，你可以用**D3**从一个数字数组中生成一个HTML表格。或者，用同样的数据来创建平滑过渡的可交互的交互式SVG条形柱状图。***[阅读更多教程](https://github.com/d3/d3/wiki/Tutorials)***  
   
**D3**不是像某个单一的框架那样旨在提供一切可以想像的功能。相反，**D3**解决了这个问题的关键所在：基于数据的文件有效操纵。这就避免了操纵文档对象的固有模式，从而获得了非凡的灵活性，把web标准的全部功能都暴露出来，如HTML，SVG，CSS。以最小的性能消耗，**D3**变得非常快，支持大型数据集和动态行为可交互以及动画。**D3** 的功能化风格允许代码通过多样化的*[组件](https://github.com/d3/d3/wiki/API-Reference)*和*[插件](https://github.com/d3/d3-plugins)*集合实现重用。

---
##选择器
---
修改使用[W3C DOM API](http://www.w3.org/DOM/DOMTR)的文档是乏味的：方法名累赘，以及遇到需要的方法时需要手动迭代还有临时状态的记录。例如，在段落元素中改变其文本颜色：

>     var paragraphs = document.getElementsByTagName("p");
>     for (var i = 0; i < paragraphs.length; i++) {
>         var paragraph = paragraphs.item(i);
>         paragraph.style.setProperty("color", "white", null);
>     }
   
**D3**采用了声明的方式，运行在人意节点集合上，称之为*选择器*。例如，你可以重写以上的循环：
>     d3.selectAll("p").style("color", "white");
诚然，你还可以根据需要操纵单个节点：

>      d3.select("body").style("background-color", "black");
  
选择器是由[W3C Selectors API](http://www.w3.org/DOM/DOMTR)所定义的，并且被现代浏览器原生支持。向后兼容旧版本的浏览器可以由[Sizzle](http://sizzlejs.com/)提供。上述例子通过标签名称来选取节点(分别是"p"和"body")。元素可以被各种谓词来选择，包括容器，属性值，类以及ID。   
   
   
**D3**为变异节点提供了多种方法：设置属性或样式；注册事件监听器；添加、移除或者排序节点；以及改变HTML或者text内容。这些就已经满足了大多数的需求。直接访问底层的DOM也是可能的，因为每个**D3**的选择器都是一个简单的节点数组。[阅读更多的选择器](https://github.com/d3/d3/wiki/Selections)
