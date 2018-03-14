> 面向对象

## 创建对象方法及其特点/优缺点

* 工厂模式
    创建多个类似对象，但是没有解决对象识别问题（怎么知道对象类型）
```bash
    function createNewObj(name, age, job){
        var o = new Object()
        o.name = name
        o.age = age
        o.job = job
        o.sayName = function(){ console.log(this.name)}
        return o
    }
    var person11 = createNewObj('John', 12, 'student')
```

* 构造函数模式
    特点：
    * 没有显示创建对象
    * 直接将属性和方法赋给this对象
    * 没有return
    缺点：
    每个方法都要在每个实例上重新创建一遍
```bash
function NewObj(name, age){
    this.name = name
    this.age = age
    this.sayName = function (){ console.log(this.name)}
}
```
* 原型模式
    * 有点
    让所有对象实例共享它所包含的属性和方法，即不在构造函数中定义对象实例的信息，而是将信息直接添加到原型对象中
    * 缺点
    遇到属性是引用对象时，实例的这些属性共享值
```bash
function Person(){}
Person.prototype.name = 'Jack'
Person.prototype.age = 22
Person.prototype.sayName = function (){ console.log(this.name)}

var person1 = new Person()
person1.sayName()               //'Jack'
```
或
```bash
function Person(){}
Person.prototype = {
    consotructor: Person,
    name: 'Jack',
    age: 22,
    sayName: function(){ console.log(this.name)}
}
```
* 组合使用构造函数模式和原型模式
    创建自定义类型最常见的方式
```bash
fucntion Person(name, age){
    this.name = name
    this.age = age
}
Person.prototype = {
    constructor: Person,
    sayName: function(){ console.log(this.name)}
}
```
* 动态原型模式
信息封装在构造函数，满足某些条件下，在构造函数中初始化原型
```bash
fucntion Person(name, age){
    this.name = name
    this.age = age
    //初次使用构造函数时才会使用，此后原型已经完全初始化，就不再需要做什么修改了
    if(typeof this.name !== 'function'){
        Person.prototype.sayName = function(){ console.log(this.name)}
    }
}
```
* 寄生构造函数模式
    * 基本思想(工厂模式？)：
    创建一个函数，函数作用仅为封装创建对象的代码，然后返回新创建的对象
```bash
fucntion SpecialArray(){
    var result = new Array()

.
    result.toPipedString = function () {
        return this.join('|')
    }
    return value
}
```

* 稳妥构造函数模式
    * 个人理解就是有私有变量和方法的工厂模式/寄生构造函数模式，且声明不用new的继承模式
    * 稳妥对象，指的是没有公共属性，而且其方法也不引用this的对象。
    * 稳妥对象适用一些安全的环境中（禁止适用this、new）或防止数据被其他应用程序改动时适用
    * 类似于寄生构造函数，不同在于新建对象实例方法不引用this、不使用new操作符调用构造函数
```bash
function Person(name, age){
    var o = new Object()
    //私有变量
    var name = 'Jack'
    o.sayName = function(){ console.log(name)}
    return o
}
var person1 = Person('Jack', 22)
person1.sayName()                   //'Jack'
```

## js继承方式及其优缺点
* 原型链
    * 问题
    同原型声明的对象。包含<b>引用类型</b>值的原型属性会被所有实例共享。
```bash
fucntion Super(){ this.property = true}
Super.prototype.getSuperValue = function(){ return this.property}

function Sub(){ this.subproperty = false}
Sub.prototype = new Super()                 //把父级实例当作Sub的原型
Super.prototype.getSubValue = function(){ return this.subproperty}
```

* 借用构造函数
    * 思想：
        在子类构造函数内部调用超类构造函数
    * 缺点：
        同构造函数声明对象。方法都在构造函数内部定义，超类方法对子类而言又不可见，复用无从谈起
```bash
function Super(){ this.color = ['red', 'blue']}
function Sub() {SuperType.call(this)}
```

* 组合继承
    * 思路：
        用原型链实现对原型属性和方法的继承，用借用构造函数实现对实例属性的继承
```bash
function Super(name){
    this.name = name
    this.color = ['red','blue']
}
Super.prototype = {
    constructor: Super,
    sayName: function(){ console.log(this.name)}
}

function Sub(name, age){
    Super.call(this, name)
    this.age = age
}
Sub.prototype = new Super()
Sub.prototype.constructor = Sub
Sub.prototype.sayAge = function(){ console.log(this.age)}
```

* 原型式继承
* 寄生式继承
* 寄生组合式继承
* ES6下的class及继承
    * 子类没有自己this，要继承父类。所以在调用super()后才能在子类中使用this，否则报错
    * super可当函数，也可当对象。
    * super函数：仅能用于子类构造函数中。代表父类构造函数，返回子类实例，相当于Parents.prototype.constructor.call(this)
    * super对象：普通方法中指向父类原型对象(Parent.prototype)，静态方法(static method)中指向父类(Parent)
    * 如果单写super，不能表明super是函数还是对象，js引擎解析代码会报错
    * es6规定，通过super调用父类方法时，方法内部this指向子类
    * 子类都有constructor方法，即使没有显示定义
    * 父类静态方法也会被子类继承

```bash
class Super{
    constructor(name, age){
        this.name = name
        this.age = age
    }
    sayName(){
        return `${this.name} ${this.age} ${super.toString()}`
    }
}
class Sub extends Super(name, age, job){
    constructor(){
        super(name, age)
        this.name = 'Rose'
        this.job = 'teacher'
    }
    sayAge(){ super.sayName()}
}
```

## 对象与继承

* es5
```bash
//test1是一条狗，叫声好听(wow)，每次见到主人就会叫一声(yelp)
function Dog(){ }
Dog.prototype.wow= function wow() {console.log('wow')}
Dog.prototype.wow= function yelp() {console.log('yelp')}
//test2原本是一个可爱的小狗，后来疯了(MadDog)，一看到人就隔半秒叫一声(wow)，不停地叫唤
function MadDog(){}
madDog.prototype = new Dog()
madDog.prototype = () => {setInterval(() => {this.wow()}, 500)}
```

> js

## 请描述一下cookies，sessionStorage和localStorage的区别？

sessionStorage、localStorage、cookie都是在浏览器端存储的数据
* 均需要同源
* sessionStorage
    sessionStorage是在同源的同窗口（或tab）中，始终存在的数据。也就是说只要这个浏览器窗口没有关闭，即使刷新页面或进入同源另一页面，数据仍然存在。
    关闭窗口后，sessionStorage即被销毁。
    同时“独立”打开的不同窗口，即使是同一页面，sessionStorage对象也是不同的。
* cookie
    大小数量有限制，远小于其余两个
    会随请求发送到服务器端。

## cookie
* 持久保存客户端数据，分担服务器存储负担
* 大小最多4kb
* 浏览器会清除cookie，且不同浏览器清除规则不一样
* 每次请求一个新页面，cookie都会被发送过去
* 优点
    * 良好编程控制cookie大小
    * 加密、安全传输技术，减少cookie被破解可能性
    * 在cookie存放不敏感数据
    * 控制cookie生命周期，使之不会永远有效

* 缺点
    * 长度数量限制
    * 安全问题，可能被拦截和转发
    * 有些状态不可能保存在客户端
    * 操作麻烦
* cookie删除
将过期时间设置在当前时间点之前

## cookie && session
* cookie存客户端，session存服务端
* cookie不安全
* session会在一定时间内保存在服务器上，访客增多时，比较占用服务器性能
* 建议
    * 重要信息存session
    * 其他要保留信息存cookie

## SVG
```bash
<svg width="100%"
        height="100%"
        version="1.1"
        xmlns="http://www.w3.org/2000/svg">
    <circle cx="100"
            cy="50"
            r="40"
            stroke="black"
            stroke-width="2"
            fill="red"/>
</svg>
```

> JS

## js垃圾回收

* 标记清除
    js最常见的垃圾回收方式。
    当变量进入执行环境时，垃圾回收器将其标记为“进入环境”，变量离开环境时，将其标记为“离开环境”。
    垃圾回收器会在运行时给存储在内存中所有变量加上标记，然后去掉环境中的变量以及闭包，在这些完成后仍存在标记的，就是要删除的变量了。
*引用计数
    低版本ie经常会出现内存泄漏
    引用计数策略是跟踪记录每个值被使用的次数，清理掉引用次数为0的值

## v8 垃圾回收机制

* 基于分代式垃圾回收机制
    v8中主要讲内存分为新生代和老生代。新生代中的对象为存活时间较短的对象，老生代中对象为存活时间较长的对象或常驻内存对象。
    v8堆整体大小就是新生代(32位16M，64位32M)和老生代(32位0.7G，64位1.4G)内存空间总和
    ##### 新生代
    主要采用scavenge算法进行垃圾回收，其具体实现中，主要采用cheney算法
    ###### cheney算法
    将内存分为二，只有一个处于使用状态（称为from空间），另一个处于闲置状态（称为to空间）。
    在垃圾回收过程中，将存活对象在两个空间之间复制
    ##### 老生代
    当一个对象经过多次复制仍存活，就会被放到老生代的空间中，称为晋升。
    晋升条件有两个
    1.对象是否经历过scavenge回收
    2. to空间的内存占用比超过限制
    老生代对象，存活对象占较大比重，所以老生代主要采用Mark-Sweep(标记清除)和Mark-Compact(标记整理)
    ###### Mark-Sweep(标记清除)
    步骤不赘述，缺点，运行完后会有很多内存碎片。此时要用Mark-Compact
    ###### Mark-Compact(标记整理)
    对象在标记死亡后，在整理过程中，将存活对象往一端移动。移动完成后直接清理掉边缘外的内存。
    #### 总结，老生代中v8主要使用标记清除，只有当空间不足以对新生代中晋升过来的对象进行分配时，才动用Mark-Compact(标记整理)

## 事件代理

* 在父元素上添加事件监听和处理函数，来处理子元素的事件，而不是直接在每一个子元素上添加事件处理函数。事件代理是因为有事件冒泡机制才成立的。
* 好处——提高性能、减少缓存

## 事件流

"捕获阶段-到达目标-冒泡阶段"。不是所有浏览器都有捕获阶段。冒泡阶段的事件是由目标元素慢慢、一层一层、一级一级向祖先元素传递，捕获阶段事件传递方向相反。

## 事件监听

* dom绑定事件--onevent
* js取得元素引用，el['onevent']
* addEventListener(event, function, bool)/attachEvent(onevent, function)

## js的this如何工作

    * this永远指向函数运行时所在对象，而不是创建时所在对象（es6的箭头函数this指向创建时所在对象）
    * 匿名函数或不处于任何对象的函数指向window。
    * 会影响this的运算符、方法有：new、call、aplly、bind、with
    * 普通函数调用，函数被谁调用，this就是谁
    * 对象中this指向其本身

## apply、call、bind

    * bind修改执行函数中this为某个指定this，但是修改后不会立即执行
    * apply、call修改后会立即执行函数

## ...
```bash
function F (){
    getName = () => {alert (1)}
    return this
}
F.getName = () => {alert(2)}
F.prototype.getName = () => {alert(3)}
var getName = () => {alert(4)}
function getName(){alert(5)}

F.getName()                 //2
getName()                   //4
F().getName()               //报错--F().getName()没有getName方法。因为F()返回一个this，this上没有getName，此时如果在浏览器中，this指向window
new F.getName()             //2 new (F.getName())
new F().getName()           //3 原型链上的继承，相当于(new F()).getName(),此时getName为原型上的getName，new F()返回一个新的F对象
new new F().getName()       //3 对象的构造函数为F.prototype.getName()。即new ((new F()).getName())

解释：
//整理后的代码，代码中有函数提升、变量提升
var getName                             //getName--undefined
function getName(){alert(5)}            //此时getName为function
//关于↑上面顺序问题，本人亲测此时getName值为函数。
function F (){
    getName = () => {alert (1)}         //getName--alert(1)
                                        //此function即为全局的function
                                        //因为没在function F中找到getName，沿着作用域链上一级查找
    return this
}
F.getName = () => {alert(2)}            //F.getName--alert(2)
F.prototype.getName = () => {alert(3)}  //原型链底端getName先于原型上的方法被找到。
getName = () => {alert(4)}              //getName--alert(4)
```


## 为什么function foo(){}()不是立即调用的函数表达式

    解析器解析全局function时，默认function为声明，而不是表达式。
    且声明非立即调用的函数表达式function需要一个名字（或者声明后立即赋给一个变量也行，此时函数name为函数名字，无则为赋值给的变量），否则浏览器会报语法错误

## 区别、检测undefined、null

## 闭包

## 匿名函数举例

(function (){})()

## js模块模式

不污染命名空间 ……

## js宿主对象和原生对象区别

* 原生对象
    即独立于宿主环境的ECMA实现提供的对象（ECMA定义的类——引用类型），
    包括Object、Function、Array、String、Boolean、Number、Date、RegExp、Error、EvalError、RangeError、ReferenceError、SyntaxError、TypeError、URIError。
* 内置对象
    即由ECMA提供的、独立于宿主换将的所有对象，在ECMAScript开始执行时出现
    Gloabl、Math，他们也是本地对象
    即内置对象是本地对象的一种
* 宿主对象
    由ECMAScript实现的宿主环境提供的对象，比如DOM、BOM

## 关于Number的建议

用js做到关于Number的问题的时候，特殊值处理请务必小心<b>NaN、Infinity</b>

## 删除数组重复元素

* hash数组
* 遍历，用indexOf判断有无重复
* 利用ES6的Set数据结构和Array.from()

## 数组反转

* 借用栈
* 头尾下标互换
* reverse

## 数组降序、排序

sort(function(){...})

## 函数节流

如果一个函数调用频率过高，则利用setTimeout延迟调用，利用闭包记录最近一次成功调用时间begin、最近一次的setTimeout--timer。调用函数后要记得更新begin、timer
```bash
//...rest为es6语法，详情请查"ECMAScript 6 入门"
function test (fn, delay, interval, ...rest){
    let timer = bull
    let begin = new Date()
    return () => {
        let cur = new Date()
        clearTimeout(timer)

        if(!begin) begin = cur

        if(cur - begin >= interval){
            fn.apply(this, rest)
            begin = cur
        } else {
            timer = setTimeout(() => {
                fn.apply(this, rest)
            }, delay)
        }
    }
}
```

## 字符串反转
参照数组反转

## 原生实现JSON.parse()、JSON.stringify()

关键点为split()、splice()
```bash
function toJsonString(object){
    let jsonStringArray = [];
    for(property in object){
        if(object.hasOwnProperty(property){
            jsonStringArray.push(`${property}: ${object[property]}`)
        }
        return '{' + jsonStringArray.join(',') + '}'
    }
}
```

## 获取浏览器url中的查询字符串参数

## 提取url中参数，并转为json格式

## 原生实现bind()

```bash
Function.prototype.bind = Function.prototype.bind || function(context){
     let self = this
     return function()
        self.apply(context, arguments)
}
```

## 定义一个log方法，让他可以代理console.log()

```bash
function log(...rest){
    console.log.apply(console, rest)
}
```

## 获取一个范围的随机整数

Math.ceil()、Math.floor()、Math.random()

## 清除字符串前后空格

* someString.trim()
* 正则、replace(/[(^\s+)(\s+$)]/g, '')


## 合并数组操作
* es6的'...'运算符 + Array.from()
* concat()

## 合并两数组，并删除第二个元素

let test = (ary1, ary2) => ary1.contant(ary2).splice(1, 1)

## 原生ajax
```bash
var xhr = new XMLHTTPRequest()
xhr.onreadystatechange = function () {
    if(xhr.readyState === 4){
        if((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304){
            console.log(xhr.responseText)
        } else {...}
    } else {...}
}
xhr.open('Get', url, true)
xhr.send(null)
```

## 变量声明提升

var声明的变量会提升到该作用域顶端。

## attribute、property区别

* property
    * 属性，比如id、title、lang、dir、className
    * 对大小写敏感
    * 值任意
    * 自定义property可出现于js中，但不能出现于html中。
* attribute
    * 特性
    * 值仅能为字符串
    * 大小写不敏感
    * 出现在innerHTML中。可通过类数组attributes罗列
* 相同点
    标准的DOM properties 与 attributes同步——公认的特性会被以属性的形式田家达dom对象中，此时操作property或getAttribute()都可操作属性，不过传递给getAttribute()的特性名要与实际的特性名先沟通相同，即传入class而不是className
* 不同点
    对于有些标准的特性操作，getAttribute()与点好获取的值存在差异，比如href、src、value、style、onclick等事件处理程序
    比如href，getAttribute()获取的是href的实际值，而点好获取的是完整的url，存在浏览器差异

## 为什么扩展js内置对象不是好的做法/为什么扩展js内置对象是好的做法
* 不好
    因为会影响到整个工程，很严重的污染。以及防止js日后实现了该方法，但与自行扩展的实现不一致，会导致程序崩溃。
* 好
    代码重用
具体做法
    vue2中对array对象进行了扩展，个人觉得可以参考下，目前尚未整理出来

## document.onload vs document.ready

* ready —— 文档结构已经加载完成，不包括图片等非文字媒体
* load —— 页面全部加载完成

## 类数组

* webpack打包原理、数数loader、热加载
工作方式：
    把项目当成一个整体，通过一个给定文件（如index.js），webpack将从这个文件开始找到你的项目的所有依赖文件，使用loaders处理他们，最后打包成一个或多个浏览器可识别的js文件
打包原理
    * 一切皆模块
    * 按需加载
loader
    * png -> url-loader
    * jsx -> babel-loader
    * style-loader、css-loader、less-loader、sass-loader、stylus-loader

* gulp
    工作方式，在配置文件中，知名对某些文件进行类似编译、组合、压缩等任务的具体步骤

* 图片懒加载及其实现（横向）

## == && ===
* ==
    * 两值类型不同，先进行类型转换再比较
    * 若值为true，转为1比较，若为false，转为0比较
    * 若为对象跟数值或字符串，调用对象toString()或valueOf()，随后进行比较。js核心内置类优先调用valueOf()、Date用toString。非js核心对象...不知道
* ===
    * 不做类型转换，类型不同一定不等

* split()、join()
* 听说nodejs有更快捷地方法，本人不了解nodejs，无法提出更好的解决方案

## js同源策略

同域名、协议、端口称为同源

## 跨域

据说常用的为cors和jsonp
* cors
    * 原理
        后台设置白名单——在返回头部中写Access-Control-Allow-Origin:被允许访问的源
    * 优点
        很方便，对前端友好
    * 缺点
        * 不能携带cookie信息
        * 不能使用自定义请求头部
        * 不兼容IE浏览器
        * b格不高
* jsonp
    * 原理
        * 在全局定义一个funName函数
        * 在页面创建script, src格式为: 'url'+'?callback=funName&key1=value1'
        * 后台定义一个借口接收cb、key等参数，逻辑计算，返回格式为字符串: funName({prop: value})
    * 优点
        * 兼容性好
        * 简单易用
        * 支持浏览器与服务器双向通信
    * 缺点
        * 只支持get请求
* document.domain
    跨子域的解决方案之一。将子域和主域document.domain设为同一个主域，但是两域名必须同一个基础域名，且所用协议、端口一致
* window.name
    window.name有个特征，一个窗口生命周期内，窗口载入的所有页面都是共享window.name的，每个页面对window.name都有读写的权限，wendow.name是持久存在一个窗口载入过的所有页面中的
* html5的postMessage()

## new

* 创建一个空对象，this指向该空对象
* 继承该函数原型
* 属性和方法被加入到this引用的对象中
* 新创建的对象由this引用
* 最后隐式返回this

## js延迟加载

* difer/async
* 动态创建dom
* 按序异步载入js
* 创建并插入iframe，让它异步执行js

## 内存泄漏

* 闭包
* 循环引用
* setTimeout第一参数是字符串而不是函数
* 控制台日志

## web应用从服务器主动推送data到客户端有哪些方法

js数据推送
* comment： 基于http长连接的服务器推送技术
* 基于WebSocket的推送方案
* SSE：服务器推送数据信访室

## flash、ajax各自优缺点

* flash
    * 适合处理多媒体、矢量图形、访问机器
    * 对css、处理文本尚不足，不容易被搜索
* ajax
    * 对css、文本友好
    * 支持搜索
    * 多媒体。矢量图形、机器访问不足
* 共同点——与服务器无刷新传递消息、用户离线和在线状态、操作dom
* ajax缺点
    * 不支持浏览器back按钮
    * 对搜索引擎支持较弱
    * 暴露客与服务器交互细节
    * 不容易调试
    * 破坏程序异常机制
* ajax请求页面历史记录状态问题
    * 通过锚点来记录状态，location.hash。让浏览器记录ajax请求时，页面状态的变化
    * 通过html5的history.pushState来实现浏览器地址栏的无刷新改变

## AMD && Commonjs

* Commonjs
    * 是服务器端模块规范
    * 同步加载
* AMD
    * 非同步加载，允许指定回调函数
    * 风格推荐返回一个对象作为模块对象，通过对 module.exports或exports的属性赋值来达到暴露模块的目的

## document.write() && el.innerHTML

* document.write()只能重回整个页面
* innerHTML可重回页面一个部分

## 代码，求字符串字节长度

getBytes = str => {
    let len = str.length
    let result = len
    for(let i = 0; i < len; i ++){
        if(str.charCodeAt(i) > 255) result ++
    }
    return result
}

## (github操作指令)git fatch && git pull

* git full
    从远程获取最新版本并merge到本地
* get fetch
    从远程获取最新版本到本地，不会merge

## MVC && MVVM

* mvc
    * view传指令到controller
    * controller完成业务逻辑后，要求modle改变状态
    * model将更新数据发送到view，用户得到反馈
* mvvm
model、view、viewModel
    * model： 数据访问层
    * view： ui界面
    * viewModel：view的抽象，负责view与model之间信息转换，将view的command传送到model

## use strict的优劣

好处
    减少怪异行为——消除js语法不合理、不严谨之处
    保证代码运行安全——消除代码一些不安全之处
    提高编译器效率
    为未来新版本js做好铺垫
坏处
    非严格模式下写的代码，在严格模式下可能不可运行

## 严格模式限制

* 函数参数不能重名，否则报错
* 不可用with
* 不可对只读属性赋值，否则抱错
* 不可用0前缀表示八进制，否则报错
* 不能删除不可删除的属性，否则报错
* 不能删除变量，仅能删除属性，否则报错
* eval不会再其外层作用域引入变量
* eval、arguments不可被重新赋值
* arguments不能自动反映函数参数的变化
* 禁用callee、caller
* 禁止this指向全局对象
* 不可用fn.caller、fn.arguments获取函数调用堆栈
* 增加保留字（比如protected、static、interface）

## 写函数实现N!
循环、递归均可，注意大数问题。可以考虑借助空间减少计算量

## n行杨辉三角

## 返回一串字符串重复最多的单词

遍历一遍，建立一个hash表，hash表中的数据形式为{单词:单词出现次数}。随后遍历hash表，找出次数最多的那个单词

## 递归打印长度为n的斐波那契数列

用数组存放计算出来的结果，省去多余的计算量

## event loop

    js只有一个主线程，主线程实行完执行栈的任务后，去循环 任务队列，如果有异步事件出发，就将其添加到主线程的执行栈

## Date()
```bash
let a = new Date()
a.getDate()             //日期，基于1
a.getDay()              //星期，[1-7]
a.getFullYear()         //年，基于1
a.getHours()            //时，基于1
a.getMonth()            //月份，基于0！！！
```

## es6中Object.assign为浅复制

## 深度克隆函数deepClone

关键点，各类对象判断、创建新对象、递归
```bash
function deepClone(obj){
    let _toString = Object.prototype.toString()
    //基础类型
    if(!obj || typeof obj !== 'object'){
        reutrn obj
    }

    //DOM Node
    if(obj.nodeType && 'cloneNode' in obj){
        return obj.cloneNode(true)
    }

    //Date
    if(_toString.call(obj) === '[object Date]'){
        return new Date(obj.getTime())
    }

    //RegExp
    if(_toString.call(obj) === '[object RegExp]'){
        let flags = []
        if(obj.global) flages.push('g')
        if(obj.multiline) flages.push('m')
        if(obj.ignoreCase) flages.push('i')
        return new RegExp(obj.source, flags.join(''))
    }

    //Object && Array
    let result = Array.isArray(obj) ? [] :
                    obj.constructor ? new obj.constructor() : {}
    for(let key in obj){
        result[key] = deffpClone(obj[key])
    }
    return result
}
```

## 完成一个函数，接收一个数组，返回一个扁平化后的数组

关键点：递归
```bash
function test(ary, result = []){
    for(let i = 0, l = ary.length; i < len; i ++){
        d = ary[i]
        if(typepf d === 'number'){
            result.push(d)
        } else {
            test(ary[i], result)
        }
    }
}
```

## 正则表达式各种匹配

6-16位英文数组密码，且英文数组均至少出现一次
/^(?![0-9]+$)(?![a-zA-Z]+$)[0-9A-Za-z]{6,20}$/gim

## (正则)把字符串"${id}${name}"中"${id}"替换为10，"${name}"替换为Tony
```bash
function test(str){
     return str.replace(/$\{{id}]}/g, 10).replace(/$\{name\}/g, Tony)
}
```

## 代码

```bash
let obj = {
    a: 1,
    b: function(){console.log(this.a)}
}
var a = 2;
var objb = obj.b
obj.b()                //1, this == obj
objb()                 //2, this == window
obj.b.call(window)     //2, this == window
```

## 代码（原型链、变量查找原则）
```bash
function A(){}
function B(a){this.a = a}
function C(a){if(a){this.a = a}}
A.prototype.a = 1
B.prototype.a = 1
C.prototype.a = 1
console.log(new A())            //A{}
console.log(new B())            //B{a: undefined}
console.log(new C(2))           //C{a: 2}
```

## 代码（闭包）

```bash
var a = 1
function b(){
    var a = 2
    function c(){console.log(a)}
    return c
}
b()()                           //2
解释：
b()相当于执行了一次b函数，返回c这个变量。(b())()就是执行一次c。最终输出a，a根据作用域的查找规则，优先取function b中的a
```

## 原生添加、移除、移动、复制、创建、查找节点

* 创建
    * document.createElement(tagName);
    * document.createTextNode(string);
* 添加 parent.appendChild(el)
* 删除 parent.removeCHild(el)
* 替换 parent.replaceCHild(newNode, oldNode)
* 插入 parent.insertBefore(childNode, ref-node/null)
* 查找
    * document.getElementById()
    * document.getElementByClassName()
    * document.getElementByTagName()
    * document.getElementByName()
    * document.querySelect()
    * document.querySelectAll()

## 实现拖拽

## 伪数组、伪数组转数组

* nodeList、arguments、HTMLCollection
* Array.prototype.slice.call(ary)

## caller、callee

* caller--返回调用此函数的函数
* callee--正在执行的function函数(arguments.callee())

## 手写快排（js）

## 代码(没声明与声明了没赋值的区别)

```bash
var a
console.log(a)              //undefined
console.log(b)              //报错
console.log(window.b)       //undefined
```

## 隐类型转换

undefined == null           //true
1 == true                   //true
2 == true                   //true
0 == false                  //true
0 == ''                     //true1
NaN == NaN                  //false
[] == []                    //false
[] == ![]                   //true
{} == {}                    //false
[] == false                 //false
{} == false                 //false
null == false               //false

## （字符串）小段杠(get-element-by-id)转驼峰(getElementById)

```bash
function test(str){
    let temp = str.split('-')
    for(let i = 0, l = temp.length; i < l; i ++){
        temp[i] = temp[i].charAt(0).toUpperCase + arr[i].slice(1)
    }
    return temp.join('')
}
```

## 代码--连续赋值

```bash
var a = {n: 2}
var b = a
a.x = a = {n: 2}        //a.x = (a = {n: 2})
                        //a = {n : 2}改变了a的地址，此时a与前面的a.x的a指向不为同一个地址
                        //但是原始的a的地址由副本b保留着，此时的b为{n:1;x:{n:2}}
console.log(a.x)        //undefined
console.log(b.x)        //{n: 2}
```

## web worker 和 webSocket

worker主线程:
    * 通过worker = new Worker(url)加载一个js文件创建一个worker，同时返回一个worker实例
    * 通过worker.postMessage(data)方法想worker发送数据
    * 绑定worker.onmessage方法来接收worker发送过来的数据
    * 可使用worker.terminate()来种植一个worker执行

webSocket是web应用程序传输协议，它提供了持久、双工、双向、按序到达的数据流，是一个html5协议。服务器可推送更新给客户端，而不需要客户端轮询

## 函数柯里化

张鑫旭的文章(http://www.zhangxinxu.com/wordpress/2013/02/js-currying/)
Haorooms的文章(http://www.haorooms.com/post/js_currying)

待整理...

## getElementById && querySelectorAll
* 前者是动态集合，查找结果根据html中页面元素实时变化
* 后者是静态集合，查找出来后，结果不会再变
* 建议：多级查找用querySelectorAll，一个查找用getElementById
