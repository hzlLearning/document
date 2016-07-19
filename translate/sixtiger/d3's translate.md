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
   
   
**D3**为变异节点提供了多种方法：设置属性或样式；注册事件监听器；添加、移除或者排序节点；以及改变HTML或者text内容。这些就已经满足了大多数的需求。直接访问底层的DOM也是可能的，因为每个**D3**的选择器都是一个简单的节点数组。[查阅更多的选择器](https://github.com/d3/d3/wiki/Selections)

---
##动态属性   
     
---
读者熟悉其他的DOM框架如[jQuery](http://jquery.com/)或者[Prototype](http://www.prototypejs.org/)，应该会立即认识到**D3**的相似之处。然而样式，属性以及其他特性，在**D3**中可以被指定为数据的功能，而不仅仅是简单的常数。尽管他们表面上看起来很简单，这些功能强大得可能会令你惊讶；比如，有一个*d3.geo.path*
函数，将项目中的[地理坐标](http://geojson.org/)作为SVG中的[路径数据](http://www.w3.org/TR/SVG/paths.html#PathData)。**D3**提供了许多内置的可重用函数和工厂函数，比如面积，折线和饼图[图元](https://github.com/d3/d3/wiki/SVG-Shapes)。   
例如，随机着色段落：   
>     d3.selectAll("p").style("color", function() {
>       return "hsl(" + Math.random() * 360 + ",100%,50%)";
>     });

要让偶数和奇数节点的灰色深浅交替：   

>     d3.selectAll("p").style("color", function(d, i) {
>       return i % 2 ? "#fff" : "#eee";
>     });

计算属性往往指的是绑定数据。数据被指定为值的阵列，并且每一个值被作为第一个参数传递到选择功能里。因为存在默认添加的索引，数据阵列中的第一个元素被传递到选择集合中的第一个节点，阵列中的第二个元素则是被传递到第二个节点，以此类推。例如，如果你把一个数字数组绑定到段落元素集合中，你可以用这些数字来动态地计算字体大小：
>     d3.selectAll("p")
>       .data([4, 8, 15, 16, 23, 42])
>       .style("font-size", function(d) { return d + "px"; });

一旦这数据绑定到了文档中，你就可以忽略这数据的运算了；**D3**将检索以前绑定的数据。这会允许你在不用重新绑定的情况下重新计算性能。

---
##Enter和Exit
---
使用**D3**的*enter*和*exit*选择器，你可以创建传入数据的新节点以及删除不再需要的传出节点。   
   
当数据绑定到一个选择器上，该数据阵列中的每个元素都会匹配到选择器中对应的节点。如果此时节点数比数据少，额外的数据元素会构成*enter*选择器，你可以通过追加到*enter*选择器中进行实例化。举个例子：
>     d3.select("body")
>       .selectAll("p")
>       .data([4, 8, 15, 16, 23, 42])
>       .enter().append("p")
>       .text(function(d) { return "I’m number " + d + "!"; });

更新节点是默认的选择－－数据运算的结果。因此，如果你忘记了*enter*和*exit*选择器，你会自动地选取仅存的并且相对应的数据元素。一个常见的模式就是要打破最初的选择并且分为三个部分：修改更新的节点，添加进入的节点，以及删除退出的节点。
>     // Update…
>     var p = d3.select("body")
>         .selectAll("p")
>         .data([4, 8, 15, 16, 23, 42])
>         .text(function(d) { return d; });
>
>     // Enter…
>     p.enter().append("p")
>         .text(function(d) { return d; });
>
>     // Exit…
>     p.exit().remove();

通过分别处理以上这三种情况，你可以准确地指定哪些操作运行在哪个节点上。这提高了性能，并且提供了过渡更大的控制权。举个例子，一个柱状图，你可能会在初始化进入时使用旧的缩放，然后在过度到新的缩放中进入柱状图并且伴随着更新以及退出柱状图。   
   
**D3**让你根据数据来转换文档；这里面同时包含了创建和移除元素。**D3**允许你在用户响应式交互，动画超时，甚至第三方的异步通知改变现有的文档对象。一个混合的方法是，在可能的情况下，文档最初由**D3**在服务器上渲染，然后更新到客户端。[查阅更多有关于数据连接的信息](http://bost.ocks.org/mike/join/)

---
##转型，而不是替代

---
**D3**并不引入新的视觉表现。不同于[Processing](http://processing.org/)，[Raphaël](http://raphaeljs.com/)，或者[Protovis](http://vis.stanford.edu/protovis/)，图形商标**D3**这个词汇直接来源于web标准：HTML，SVG，以及CSS。举个例子，你可以使用**D3**创建SVG元素并且用外部样式表来定义它的风格。你还可以使用复合滤镜效果，虚线笔触以及剪裁。如果浏览器厂商在明天推出了新的功能，你立即就能够使用他们－－无需工具包更新。而且，如果你决定在未来使用**D3**以外的工具包，你也可以将你学到的标准知识带在身上。    
    
最重要的是，**D3**容易使用浏览器内置的元素进行检查调试：你使用**D3**来操纵的节点与浏览器本身所理解的是完全相同的。

---
##过渡
---
**D3**的关注点在于将自然延伸变换到动画过渡。随着时间的推移，视线中的样式和属性会逐渐增加。渐变可以通过平滑的功能，比如"elastic"，"cubic-in-out"以及"linear"来控制。**D3**的内插器既支持初级的，比如数字和嵌入字符串中的数字(字体大小，路径数据等等)，也支持复合的值。你甚至可以扩展**D3**的内插器注册表来使其支持复杂的特性和数据结构。   
   
举个例子，将页面背景褪色成黑色：
>     d3.select("body").transition()
>       .style("background-color", "black");

或者，使用交错延迟的方式，将符号集合映射到圈圈上来调整圈圈大小：   
>     d3.selectAll("circle").transition()
>       .duration(750)
>       .delay(function(d, i) { return i * 10; })
>       .attr("r", function(d) { return Math.sqrt(d * scale); });

通过修改那些真正能够改变的属性，**D3**降低性能消耗并且在高帧率下允许更大更复杂的图形。**D3**还可以通过事件来进行复杂的过渡测序。而且，你仍然可以使用*CSS3*的过渡方法；**D3**不会取代浏览器的工具栏，但是会用一种方式将其暴露出来，使得我们更容易使用。   
   
想学习更多？[请查阅这些教程](https://github.com/d3/d3/wiki/Tutorials)。