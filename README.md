# js-bug-
我们任何一个人都少不了要去调试代码，这件事情无分水平高低，调试程序都是一项必不可少的工作。但是修复bug和定位bug的能力，却是能突显一个人水平高低的重要体现。

我们自己做的页面 一旦出现了问题 我们能够很快定位到问题发生的地方，那是因为我们了解自己写的整个业务逻辑；相反当不是自己写的脚本或者代码出现了问题 调试起来难度就很大了，这方面就需要足够的技巧和方法，否则别人几分钟搞定的，你几小时也搞不定 工作效率各种下降。

### 技巧方法

#### 一、遇到错误怎么办

工具篇的问题 不想再多说 chrome firebug fiddler 这些都是常用且功能强大的东西 对于我们解决bug非常重要 大家应该都会
bug分类：事件驱动型、流程驱动

**1、调整心态**

> 出了问题，先自己分析问题，理清楚大概思路，理清楚自己要询问问题的点，再找人解决问题，这样除了有助于更快解决问题，同时也有助于自己的提高。

**2、寻找特征**

> 定位问题，发现问题发生在哪里，对于简单的报错信息完全可以利用调试工具的提示：xxxx行出现什么什么错误。但是对于IE下的调试 有时候经常提示不是会很准确，无法通过行数进行定位问题，那么就接着往下看：

我们遇见的错误表象一般分为以下几种：
- **直接性的语法报错**，例如XX is undefined…或者 少个分号 多个什么符号之类的，这些内容一般都会有非常明确的代码行数标示，所以不需要多说

- **报错，但是地点不是问题真正的根源所在**，例如：**某个地方报错，但是实际上之前其他行哪里的结算错误，导致后面的一些报错**；这类报错相对比较隐性一点，找起来会比较麻烦 后面会介绍一些方法，归结怎么解决这类问题（iscoll的一些bug）

- **js无报错，但是功能无效，效果出不来**：这类问题 最糟糕，也往往让人很抓狂，但是这类问题往往最终解决错误的方法更容易，可以根据特征 快速定到大概位置

####二、遇到错误怎么办

**1、化繁为简（排除法）**

**适用于**：莫名其妙的报错，不方便调试，单纯看代码无法解释出错原因。

**主要表现**：js脚本冲突、编码问题、DOM ID冲突，兼容性bug等

**排查方法**：这个是通常大家用的方法，这类办法 属于比较笨的办法，但是也是极其有效的一种办法，通过删减代码片段 反复刷新页面，直到症状消失，最后一次删除的区域很可能就是导致错误的根源，进一步查找根源可以在目标区域使用更小单位的区域定义反复使用此方法。

**PS**：对于确认与相关功能有关的部分切忌不要随便删除。

**2、顺藤摸瓜**

**适用于**：错误信息较准确，能够按照提示的错误逐层跟踪，使用“化繁为简”能够基本定位的错误

**主要表现**：基本语法错误、逻辑错误、不严谨（最常见的有：数组下标越界，null空指针导致的对象找不到，undefined未初始化，NaN数字计算错误等）

**排查方法**：这类问题一般高级浏览器下也会同时报错，我们可以利用F12，从问题对应行数开始，像上反复查找错误处的执行和调用，结合断点调试，能更快定位bug

**忌**： 钻牛角尖！如何长时间仍无法找到原因 说明这并不是一个准确的错误信息，肯定有其他潜在的因素在产生错误。这时候考虑下一种方法

**3、反复对照**

**适用于**：部分浏览器报错、事件响应异常、js操作DOM无效、PC各浏览器功能不一致等

**主要表现**：各种兼容问题

**排查方法**：这类问题 出现 是需要花点体力活，
A：检查可疑代码，细化功能点，改一点测试一点 直到发现导致错误的关键代码。
B：抽离出问题，制作简易demo，实现简单的需求 当功能正常后，与出错的正式代码进行比较，必要时进行功能代码替换 换一种写法来测试
C：代码是死的 人是活的 换一种实现方法 没必要再一颗树上吊死（ubb问题，IE11 bug）

**忌**： 急躁、马虎

**4、积累经验**

**适用于**： 应用普通方法很难定位错误，前两种方法怎么用都还是找不到头绪。

**主要表现**：逻辑复杂、功能互相绑定难以剥离、页面对象内容复杂、有的页面正常有的页面不正常、兼容问题等

**排查方法**：这类问题 是需要比较强的经验积累才能做的比较好 初期 可以通过百度 google 描述bug症状 看网上是否有相应的解决bug的方法，通过多学多看 多查找，总结更多的经验

**忌**： 不求甚解

####三、常见bug整理

兼容的bug 这里就不一一描述，日常要多去了解总结，主要说一些更直观实际的问题

**1、基本语法问题**

单双引号、分号、逗号、拼写错误、不匹配的引号、圆括号或花括号等问题。

主要一个坑是，对象式写法里，对象里的最后一个属性或者方法 不能加 “逗号”，否则IE6提示报错或者不报错或者无报错，但无法响应任何操作。如图所示：



在某个对象结尾处，多加了一个逗号，这种问题 多出现在任意添加或者删减方法的时候 忘记删除或者添加逗号引起，那么其他浏览器下正常，IE6下会报如下错误


1、 条件判断上出现的问题

不要犯无意地使用赋值运算符的错误：把第二个参数的值赋给第一个参数。因为它是一个逻辑问题，它将一直返回true且不会报错。
``` js
if(var1 = var2){} //返回true。把var2赋值给var 这不是等于判断 这是赋值操作，所以各浏览器下不会报错
```

**2、charset引起的编码错误**

如果发现IE6报错 之后 根据行数无法准确定位到一行代码，那么优先考虑编码问题。


主要出现在IE6，除IE6之外的浏览器，会根据外链脚本的不同来自行自动转换编码方式，而IE不会，一旦你页面用的是gbk，而某一个脚本是utf-8的编码就会报错，这时候需要通过在script指定编码 进行解决，IE6独有的bug
``` 
<script ssrc="XXX.js" charset="utf-8">
```



**3、XXX is undefined！某东西未定义**

这种坑一般出现在两个方面：

A、就是你真的没定义变量或者函数 就直接调用，不管是没写或者写错 js都会认为是一个你用的是一个全新的未定义的变量

B、还有一种就是作用域引起的明明定义了，却提示未定义！这种情况首先考虑修改脚本的位置，其次终极办法 就是把这个变量或者函数 赋值给全局对象window，window是可以全局共享的
``` 
(function(){    
	var a=123;    
	// window.a=a; 可以通过这句话来修改
})()
alert(a);//会报错！ ReferenceError: a is not defined 
```

**4、页面不报错，但是效果乱了**

这种问题 对大家的业务逻辑有一定考验能力，不过虽然不能100% 准确 但是却还是有章可循！
A、Jquery的使用 JQuery默认在查找不到元素的时候 也是不会报错了，如果是针对JQuery封装的一系列操作 默认元素不存在 也不会报错 只是不执行而已 这是Jquery内部的一套容错机制，所以可以从最最初元素查找上看，是否某个特定的元素标签 没有定义
B、脚本内部 对错误执行一种规避方式，经常会看到类似 代码：

``` 
 //当XX未定义 或者不存在时    
 if (!op.container) return false；    //如果XX定义 或者存在时，才执行里面的代码    
 if（op.container）{        
	 //下面的代码语句......    
 }    
```
那么我们要关注 一些业务逻辑块中 初始定义的 return 或者if语句，可能会因为元素为定义或者不存在代码直接停掉了 没有向下执行

C、脚本逻辑错误，这些说起来就比较多了！
比如在一些数据的加减乘除运算中 由于计算问题 出现NaN的情况 导致脚本的一些效果 偏离预想；

比如 判断错误（多出现在一些未考虑到的极限判断情况） ，导致逻辑进入错误的代码块分支 导致页面效果出错

**5、无法获取到某个未定义元素的属性**


这类问题如果出现 比较好处理，一般可以根据该元素 反向逐层查找，很容易找到

有可能是， for循环 闭包踩坑的问题，导致i值始终是最终退出循环的值，导致数组null值始终找不到属性（数组下标越界）

null空指针导致的对象找不到，undefined未初始化，NaN数字计算错误等等问题

**6、跨域请求的问题**

这问题有点大 有机会单独讲，


类似这样的bug提示 说 同源策落导致XMLhttprequest对象无法加载这个url，，因为域名源是v23.pclady.com.cn/XXXXX，
这种问题 一般在ajax请求的时候会出现，所以相对比较好定位

**7、另说一种情况**

除此之外，说一个某些同学会遇到的特殊情况，写JS脚本的时候 一定要注意代码的执行顺序，你放置代码的位置有可能导致 你的业务出现错误，这种坑我见有人踩过 切记！

一篇分享不能详尽所有情况，也只是说了一些比较基本的东西。但是只要大家要多总结，多整理 有耐心，bug其实也没那么可怕

