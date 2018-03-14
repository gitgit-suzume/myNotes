> ES6

待补充简要说明，emm我又找不到当年快速入门的教程了……

* let、const
    注意无变量提升、不可重复声明、const存储的值不可改变
* 模板字符串
* 箭头函数
    * this指向定义时所在对象
    * 不可用作构造函数
    * 无arguments
    * 不可用作Generator函数
* for-of、Interator接口
* promise、Generator函数
    promise状态
    * pendding——初始状态，非reject、fulfilled
    * fulfilled——成功
    * reject——回绝
    * settled——已fulfilled或reject，且不是pendding
```bash
//构造promise
let pro = new Promise(fucntion(resolve, reject){
    //succeed
    if(...){resolve(result)}
    //fails
    else{reject(err)}
})
pro.then(onFulfilled, onReject).catch(err => {console.log(err)})
```
* ...rest(arguments的代替品，是个数组)
* 解构赋值
* Symbol--js第六个新的基本类型
* class语法、class继承
* Generator
