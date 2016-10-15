# mac上安装spacemacs简易教程
## 你需要什么？
首先你要有一台mac。
安装homebrew：这个是个好东西。看看他官网上面一条安装指令非常的简单：
```shell
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```  
安装的时候可能很慢是网速的问题，选择网络环境比较好的时候安装。然后耐心等待。  
（ps:我之前因为网络问题重新安装了好几次 T_T ）  
之后你必须安装git。虽然可以看到这篇文章的人肯定是安装过git的但是保险起见。我还是说了一遍。这个mac上太好装了不说了。  
对！这就是全部准备工作。
## 安装emacs-for-mac
可能你已经聪明的找到了spacemacs的官网。然后看到了上面有关emacs咋sox上安装的介绍：   
用brew安装emacs-plus哈哈！恭喜你遇坑了！我就是这样安装的。结果出现了下方状态栏出现色差的问题。  
在一位群内好友的帮助下我成功的解决了这个问题;完美的解决方案在这里  

https://github.com/railwaycat/homebrew-emacsmacport/blob/master/README.md

## 安装spacemacs了
之后就可以安装这个spacemacs。其实很简单在github上面clone下来，更换你根目录里`.emacs.d`就可以了。这里附上github的地址（ps：这个真的可以百度出来）  
    https://github.com/syl20bnr/spacemacs
现在你在开启时就会安装这个spacemacs了

## 了解更多
其实spacemacs的文档很强大的，不过都是英文。如果你们谁找到了中文版的可以推荐给我。  
这里我推荐*子龙山人*的视屏教程。还可以加入emacs的中文bbs论坛。
