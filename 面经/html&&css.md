> HTML && CSS
## 描述一下渐进增强和优雅降级之间的不同

* 优雅降级
web站点在所有版本较新的浏览器都能工作，如果用户使用老旧版本浏览器，代码会检查确认能否在此环境下正常工作，针对不同版本的IE使用hack降级,为无法支持功能的浏览器提供候选方案，使之在旧式浏览器上以某种形式降级体验却不至于完全失效.
* 渐进增强
从被所有浏览器支持的基本功能开始，逐步地添加那些只有新式浏览器才支持的功能,向页面增加无害于基础浏览器的额外样式和功能的。当浏览器支持时，它们会自动地呈现出来并发挥作用。

## 大量图片的网站加载优化
* 图片懒加载
* 图片预加载
* 加载缩略图
* 图片展示区与图片大小保持一致

## 浏览器内核
    * IE--trident
    * Firefox--gecko
    * Safari--webkit
    * Opera--Blik
    * Chrome--Blick(基于webkit)

## div布局较table布局有什么优势

* 表现与结构分离
* 页面加载速度更快，结构更清晰，页面显得更整洁
* 易于优化，对搜索引擎更友好

## html一些相似的标签
* img && title
* strong(语义化，表重点) && b(仅提供样式)
* em(语义化，表强调) && i(仅提供样式)

## src && href
* src
    * 指外部资源的位置，指向内容将会嵌入到文档中当前标签位置
    * 加载会产生阻塞，浏览器解析该元素时候，会停止其他资源下载和处理，直到该资源加载、编译、执行完毕。
* href
    * 指网络资源所在位置，建立和当前元素（锚点—）或当前文档（链接）之间的链接。浏览器会并行下载资源，且不会停止对当前文档的处理。


## 线程与进程区别

* 一个程序至少有一个进程，一个进程至少有一个线程。线程的划分度小于进程
* 进程拥有独立的内存单元，但是多线程共享内存
* 进程有独立的程序出入口、顺序执行序列。线程不可独立执行，必须依存于应用程序中，由应用程序提供多个线程的控制
* 操作系统调度、管理、分配资源是根据进程而不是线程

## 说说你对语义化的理解？

* 去掉或样式丢失的时候能让页面呈现清晰的结构：
    html本身是没有表现的，部分标签样式如h标签其实html默认的css样式在起作用，所以去掉或样式丢失的时候能让页面呈现清晰的结构不是语义化的HTML结构的优点，但是浏览器都有有默认样式，默认样式的目的也是为了更好的表达html的语义，可以说浏览器的默认样式和语义化的HTML结构是不可分割的。
* 屏幕阅读器（无障碍访问网页）。
* PDA、手机等设备可能无法像普通电脑的浏览器一样来渲染网页（通常是因为这些设备对CSS的支持较弱）。
* 有利于SEO
    和搜索引擎建立良好沟通，有助于爬虫抓取更多的有效信息：爬虫依赖于标签来确定上下文和各个关键字的权重。
* 利于团队开发与维护

## 你如何对网站的文件和资源进行优化？

* 文件合并文件最小化
* 文件压缩
* 使用CDN托管缓存的使用（多个域名来提供缓存）
* 其他。

## 请写一个简单的幻灯效果页面
除了js完成外，可以考虑用纯css完成，具体请自行查阅

## 请谈一下你对网页标准和标准制定机构重要性的理解。

让浏览器兼容性问题尽量小，先约束浏览器开发者，后约束开发者

## 什么是FOUC（无样式内容闪烁）？你如何来避免FOUC？

    IE会先加载整个HTML文档的DOM，然后再去导入外部的CSS文件，因此，在页面DOM加载完成到CSS导入完成中间会有一段时间页面上的内容是没有样式的。这段时间的长短跟网速，电脑速度都有关系。
    解决方法：在<head>之间加入一个<link>或者<script>元素就可以了。

## doctype（文档类型）的作用是什么？你知道多少种文档类型？

此标签可告知浏览器文档使用哪种HTML或XHTML规范。该标签可声明三种DTD类型，分别表示严格版本（Strict）、过渡版本（Transitional）以及基于框架的HTML文档（Frameset）。
* Standards（标准）模式（也就是严格呈现模式）用于呈现遵循最新标准的网页
* Quirks（包容）模式（也就是松散呈现模式或者兼容模式）用于呈现为传统浏览器而设计的网页。

## 浏览器标准模式和怪异模式之间的区别是什么？（box-sizing）

* 来源
    W3C标准推出以后，浏览器都开始采纳新标准，但存在一个问题就是如何保证旧的网页还能继续浏览。
    在标准出来以前，很多页面都是根据旧的渲染方法编写的，如果用的标准来渲染，将导致页面显示异常。
    为保持浏览器渲染的兼容性，使以前的页面能够正常浏览，浏览器都保留了旧的渲染方法（如：微软的IE）。
    这样浏览器渲染上就产生了Quircks mode和Standars mode，两种渲染方法共存在一个浏览器上。
* box-sizing
    IE盒子模型(border-box)和标准W3C盒子模型(content-box)：ie的width包括：padding\border。标准的width不包括：padding\border
    js对这两个模式的判断依据为：document对象的属性compatMode——BackCompat对应quirks mode，CSS1Compat对应strict mode。

## box-sizing属性
* content-box：默认属性，标准和模型，width = content，Width = border + padding + content
* border-box： Width = width = border + padding + content
* padding-box:

## 常见兼容问题
* png24位图片在ie6浏览器上出现背景——做成png8,、或引用一段脚本处理
* 浏览器默认的margin、padding不同——*{margin: 0 0; padding: 0 0;}
* ie6装编剧bug：块属性元素float后，又有横行margin请胯下，在ie6显示margin比设置大
* 渐进识别
```bash
css
.class-name{
    background-color: red;
    .background-color: yellow: /*ie678*/
    +background-color: black; /*ie67*/
    _background-color: pink； /*6*/
}
```

## 上下margin重合问题
ie、ff中，相邻两元素如有左右margin，不重合，但是上下margin重合

## XHTML vs HTML？
区别：
* XHTML 元素必须被正确地嵌套。
* XHTML 元素必须被关闭。
* 标签名必须用小写字母。
* XHTML 文档必须拥有根元素。

局限：
* 所有的 XHTML 元素都必须被正确地嵌套
* XHTML 必须拥有良好的结构
* 所有的标签必须小写
* 所有的 XHTML 元素必须被关闭
* 所有的 XHTML 文档必须拥有 DOCTYPE 声明
 * html、head、title 和 body 元素必须存在
 虽然代码更加的优雅，但缺少容错性，不利于快速开发。

## data-属性的作用是什么？

    data-* 属性用于存储页面或应用程序的私有自定义数据。data-* 属性赋予我们在所有 HTML 元素上嵌入自定义 data 属性的能力。存储的（自定义）数据能够被页面的 JavaScript 中利用，以创建更好的用户体验（不进行 Ajax 调用或服务器端数据库查询）。
data-* 属性包括两部分：
    * 属性名不应该包含任何大写字母，并且在前缀 "data-" 之后必须有至少一个字符
    * 属性值可以是任意字符串

## rest.css文件作用和好处

    使浏览器默认样式统一

## 解释下浮动和它的工作原理。

    浮动的框可以向左或向右移动，直到它的外边缘碰到包含框或另一个浮动框的边框为止。
    要想使元素浮动，必须为元素设置一个宽度（width）。
    虽然浮动元素不是文档流之中，但是它浮动后所处的位置依然是在浮动之前的水平方向上。
    由于浮动框不在文档的普通流中，所以文档的普通流中的块框表现得就像浮动框不存在一样，下面的元素填补原来的位置。
    有些元素会在浮动元素的下方，但是这些元素的内容并不一定会被浮动的元素所遮盖，
    对内联元素进行定位时，这些元素会考虑浮动元素的边界，会围绕着浮动元素放置。
    也可以把浮动元素想象成是被块元素忽略的元素，而内联元素会关注浮动元素的。

## 列举不同的清除浮动的技巧，并指出它们各自适用的使用场景

* 使用空标签清除浮动。在所有浮动标签后面添加一个空标签定义css clear:both.弊端就是增加了无意义标签。
* 父标签元素添加：overflow:auto;zoom:1/*用于兼容ie6*/;。
* 使用after伪对象清除浮动。该方法只适用于非IE浏览器。注意：
    该方法中必须为需要清除浮动元素的伪对象中设置height:0，否则该元素会比实际高出若干像素；
    content属性是必须的，但其值可以为空，content属性的值设为”.”，空亦是可以的。
* 浮动外部元素

## css hacks

```bash
* background-color:red\9;
    \9所有的ie浏览器可识别；

* background-color:yellow\0;
    \0是留给ie8的
* +background-color:pink;
    +ie7定了；

* _background-color:orange;
    _专门留给神奇的ie6；

* :root#test{background-color:purple\9;}
    :root是给ie9的

* @media all and(min-width:0px){
        #test{
            background-color:black\0;
        }
    }
    这个是老是跟ie抢着认\0的神奇的opera，必须加个\0,不然firefox，chrome，safari也都认识。

* @media screen and(-webkit-min-device-pixel-ratio:0){
    #test{
        background-color:gray;
    }
  }
    最后这个是浏览器新贵chrome和safari的。
```

## 媒体查询

```bash
@media 设备名 only (选取条件) not (选取条件) and (设备选取条件)，设备二{sRules}。
```

## 隐藏元素

* display:none;
    缺陷:搜索引擎可能认为被隐藏的文字属于垃圾信息而被忽略屏幕阅读器（是为视觉上有障碍的人设计的读取屏幕内容的程序）会忽略被隐藏的文字。
* visibility:hidden;
    看不见摸不着，但是占着自己原有的空间
* overflow:hidden;
* 宽高设零

## 网页打印样式

* link的media属性为print，其属性还可以为显示器screen、电视tv、投影仪projection
* print打印样式请注意
    * 无法打印css背景图片
    * 无法打印float属性元素
    * 单位用pt、cm，而不是px
    * 隐藏{@print div{display: none}}

## 高效的css
* 样式从右向左解析
* 解析速度：id > class > tag > universal(*)
* 不要写 li #test
* 后代选择器低效
* css3效率问题（nth-child(n)等会浪费浏览器资源）
* 注意可读性、可维护性

## css解析规则

从右往左解析，解析过程会生成一个集合，初始集合一般由最后一个索引产生，慢慢向左/前解析索引，解析过程中会慢慢把不符合条件的元素从集合中剔除，最后剩下来的集合即为满足条件结果

## css预处理器

less、sass、stylus
* less 学习成本低
* sass 学习成本高，缩进式语法，在提供特性上占有优势
* stylus 功能更为强壮，与js联系紧密，有专门js api，支持ruby之类的框架

## 设计中使用了非标准字体

* 图片代替
* web fonts在线字库
* @font-face, Webfonts

## display
* none
* block
* inline
* inline-block
* flex
* table
* inline-table
* table-cell
* 等等

## css样式设置三种方法

## css选择器种类，及优先级

## css隐藏dom元素
* display:none;
* visibilty: hidden;
* width: 0; height: 0;
* opacity: 0;
* z-index: -1000

## 行内元素、块状元素、inline-block

## 外边距重叠
* 两个相邻的外边距均为正数时，结果为较大者。
* 均为负数，结果为绝对值的较大者
* 一正一负，结果为相加和

## rgba vs opacity
* opacity可继承，rgba不可。
* opacity作用于元素，rgba作用于颜色、背景色

## 居中元素
* display: felx; align-item: center; justify-content: center;
* margin: 0 auto; /*仅可水平居中*/
* margin/padding 设置上下、左右相同的值
* line-height: 固定数值，百分比不行; text-align: center;
* dispaly: table-cell; vertical-aligin: center;
* display: box; box-aligion: center; /*适用于没有固定宽高的*/
* position
* transition

## px、em、rem
* px是固定值，为一个像素
* em会继承父级元素的字体大小
* rem(r指root)的参考值是html上的字体大小，假设html{font-size: 100px},那么.1rem为10px

## 移动端媒体查询、dpr、rem、vw
* vw指屏幕宽度百分比，vh指屏幕高度百分比，目前兼容性不太好
* rem 指相对于根元素的字体大小的单位，比如在html中设置font-size: 10px
* 一旦根节点html定义的font-size变化，name整个网页运动到的rem也会变化
* 缺点：
    * rem通常不用于字体，因为不同的rem计算方式会缠身很多奇怪的大小，使得文字出现糊掉或者模糊的情况。
    * 在app里native界面与web界面混合使用时，rem在不同尺寸屏幕上的适配不一致
    * 屏幕大点字就大点，屏幕小点字就小点。但是现在量大平台设计哲学是希望无论屏幕大小（我用A3还是A4），肉眼看到的字体都是一样大的
* 结论
    * rem处理适配距离问题，em处理文字大小

## 行内元素、块级元素、空元素
* 行内元素
    span、img、input、textarea、em、a、button、select、label
* 块级元素
    div、ul、ol、li、table、h1-6、p、dl、dt、dd
* 空元素
    br、img、meta、link、hr

## css hack

* .+*#          ->      ie6、 7
* .-_           ->      6
* \9            ->      6、7、8、9、10
* \0            ->      8、9、10
* \9\0          ->      9/10
* !important    ->      7、8、9/10

## css的flex下一些常用的属性和属性值

## 左右布局，右边自适应
    * 左边float:left；右边：overflow: auto;/width: 100%;
    * body{display: flex} .right{flex: 1}
    * 左右布局，右边width:100%，左边float上来，右边设置上margin。