## JS相关

1. #### ES5和ES6的继承方式区别？

	- ES5 以函数形式定义类，以prototype 来实现继承
	- ES6 以class 形式定义类，以 extend  来实现继承

2. #### ES6模块引入和 CommonJS的区别？

   ES6 模块化：`import`用于引入模块; `export`模块的对外接口。

   - 不可重新赋值否则会编译报错。
   - 对模块的引用外部 , 可以拿到实时的值。

   CommonJS 模块化： `require` 用于引入模块；`module.exports` 模块的对外接口

   - 可重新赋值.
   - 对模块的浅拷贝 , 是缓存的值。

3. #### 严格模式和非严格模式的区别？

   有很多，举一些：

   - 严格模式变量必须声明，否则报错。（正常模式不声明，默认是全局变量）
   - 严格模式禁止 this 关键字指向全局对象， 全局中的函数this 为 undefined. (正常模式this 指向全局对象 window)
   - 严格模式有些对象的属性是禁止删除和新增的，进行操作会报错。（正常模式则不会报错）
   - 严格模式拥有多个同名的属性会报错。（正常模式取最后一个）

4. #### let const var 对比

   块 `{}` 是if for 里面的  `{}`,函数的 `{}` 不叫块。

   var 特点 ：1，可重复声明。 2，无限制修改。 3，没有块级作用域。（在块｛｝里面声明，块外边依然可拿到，解决闭包问题，需要声明一个自执行函数解决）

   let特点： 1，无法重复声明。 2，变量可修改。 3，有块级作用域。( var 改 let 解决经典闭包问题)

   const特点： 1，无法重复声明。 2，常量，不可修改。 3，有块级作用域。

5. #### 对原型链的理解？ `prototype` 上有哪些属性？

   原型链:

   - 原型链是一种机制，指的是 js中在创建对象的时候，都有一个内置的`__proto__`属性，指向创建它的函数对象（js中函数也是对象）的原型对象（即`prototype`）。因为原型对象(`prototype`)中也有 `__protot__`因此在不断指向中，形成了原型链。

   - js里完全依靠"原型链"(prototype chain)模式来实现继承。
   - 原型链的形成是真正是靠 `__proto__` 而非 `prototype`原型对象。

   ` prototype `上的属性：

   - `constructor`  用来引用它的函数对象。是一种循环引用
   - `__proto__`

   

   原型链的内存结构：（补充可不用）

   ```js
   function F(){
       this.name = 'zhang';
   };
   var f1 = new F();
   var f2 = new F();
   ```

   

   ![图片描述](https://segmentfault.com/img/bVwFw5)

   实际运用：(补充可不用）

   ```js
   function F(){};
   F.prototype = {
       constructor : F,
       doSomething : function(){}
   }
   //加上 constructor 是因为重写了原型对象constructor属性就消失了，需要手动补上。
   ```

   

6. #### 为什么要使用继承？

   面向对象编程。继承的意义在于扩展，对付编程最大的敌人重复代码，抽象出父类，子类实现继承扩展，类似迭代版本。

   平时用到的继承，比如 vue 的mixin 混合js。**可以减少重复代码，便于维护，持续迭代版本**。父类要尽量抽象，继承父级的属性和方法，可以在原有基础上扩展，还可以重写父级方法。

7. #### 手写实现4种继承

   **原型链继承**：

   核心：将父类的实例作为子类的原型

   ```js
   function Person(name,age) {
       this.name = name || "unknown";
       this.age = age || "0";
       this.hobbies = ['music','reading']
   } //父类：人
   Person.prototype.say = function(){ console.log("我是一个Person")}
   function Student(name) {
       this.name = name;//需要在子类中处理，不能复用父类
       this.grade = "三年级";
       this.age = "25"
   } //子类：学生
   Student.prototype = new Person("Student","2") //以子类的prototype 设置为父类的实例方式继承
   Student.prototype.constructor = Student// 所有涉及到原型链继承的继承方式都要修改子类构造函数的指向，否则子类实例的构造函数会指向 Person。
   var student = new Student("小明"); //子类构建实例时,不能向父类传递参数   
   var student2 = new Student("小白"); 
   student.hobbies.push('basketball')
   student.say()//输出：我是一个Person。  父类方法可复用,
   console.log(Student.prototype)
   console.log(student2.name,student2.grade,student2.age)
   console.log(student2.hobbies)// ["music", "reading", "basketball"] 引用类型被迫改变
   ```

   优点：父类方法可复用。

   缺点：1，父类的引用类型属性被子类共享(一改具改)。 2，子类构建实例时 , 不能向父类传递参数.需要单独自己处理。

   **构造函数继承：**

   核心：将父类构造函数的内容复制给了子类的构造函数。这是所有继承中唯一一个不涉及到prototype的继承。

   ```js
   function Person(name,age){
       this.name=name;
       this.age=age;
       this.hobbies = ['music','reading']
   }
   Person.prototype.say = function(){ console.log("我是一个Person")}
   function Student(name,age,grade){
       Person.call(this,name,age);//创建子类实例的时候，向父级传参了，然后交于父类处理
       this.grade=grade;
   }
   var stu = new Student('xiaoming','18','高三');
   var stu2 = new Student('xiaoli','17','高三');
   stu.hobbies.push('basketball')
   stu.say()//报错：stu.say is not a function。父类方法不可复用
   console.log(stu2)
   console.log(stu2.hobbies)// ["music", "reading"] 引用类型没有改变
   ```

   优点：1，父类的引用属性不会被共享。 2，子类构建实例时可以向父类传递参数

   缺点： 1，父类的方法不能复用，子类实例的方法每次都是单独创建的。

   **组合继承**：

   核心：原型式继承和构造函数继承的组合，兼具了二者的优点。

   ```js
   function Person(name,age){
       this.name=name;
       this.age=age;
       this.hobbies = ['music','reading']
   }
   Person.prototype.say = function(){ console.log("我是一个Person")}
   function Student(name,age,grade){
       //第二次调用 Person
       Person.call(this,name,age);//创建子类实例的时候，向父级传参了，然后交于父类处理。
       this.grade=grade;
   }
   Student.prototype = new Person();//第一次调用 Person
   var stu = new Student('xiaoming','18','高三');
   var stu2 = new Student('xiaoli','17','高三');
   stu.hobbies.push('basketball')
   stu.say()//可运行，父类方法可复用
   console.log(stu2,"stu2")
   console.log(stu2.hobbies)// ["music", "reading"] 引用类型没有改变
   ```

   优点：1，父类方法可复用。 2，父类的引用属性不会被共享  3，子类构建实例时候可向父类传递参数

   缺点：调用两次父类构造函数，造成性能浪费。

   **ES6 Class extends**

   核心：ES6继承是ES5继承的语法团。

   - **super是什么？**  super是一个函数，子类中的super就是父类中constructor构造器的一个引用；
   - **什么时候需要使用super？**  子类需要有自己 独有 的属性时，就需要在子类的`constructor`构造器内部使用super函数。

   因为继承时父类对象的属性和方法 加到子类的`this`上面( 所以必须先调用 `super`方法 ），然后子类的构造函数修改 `this`。

   ```js
   class Person {
       constructor(name,age) {
           this.name = name;
           this.age = age;
       }  
       error(err){
           console.error(err)
       }
   }
   
   class Student extends Person{
       constructor(name,age,number){
           super(name,age);
           this.number = number;
       }
   }
   
   let stu = new Student('小王',23,"2009001")
   stu.error("报错了")//报错了
   console.log(stu) //{name: "小王", age: 23, number: "2009001"}
   ```

   

8. #### for in 和 for of、forEach区别?

   `for in` 适合遍历对象，因为遍历数组的话会遍历数组原型上的属性和方法。

   `forEach` 不支持 `break` `continue` `return` 

   `for of `遍历数组不会遍历其索引，而是值，不会遍历原型

   `for in ` 可以遍历到对象的原型的方法和属性,可在循环中判断过滤掉。（hasOwnPropery方法可判断属性思否是该对象的实例属性）

9. #### JS实现并发控制

   使用消息队列以及`setInterval`或`promise`进行入队和出队

10. #### 闭包

    - 为什么要用闭包？  JS链式作用域的结构导致 父级变量无法访问子级变量。
- 概念： 内层函数能够访问外层函数作用域的变量。
  
- 创建方式：在函数创建自执行函数。
  
- 缺点：常驻内存，会增大内存使用，使用不当造成内存泄漏。
  
- 作用： 
  
    - 使用闭包修正打印值
        - 实现柯里化。f(x,y){x+y} 变 f(x)(y)
        - 实现 node commonJs 模块化，实现私有变量
        - 保持变量与函数活性，可延迟回收和执行
    
11. #### Promise 优缺点？

    优点：

    1，用同步（书写方便）的方式去书写异步代码（性能好）避免了层层嵌套的回调地狱。

    2，`Promise` 对象提供统一的接口，使得控制异步操作更加容易。

    缺点：

    1， `Promise` 一旦建立就会立即执行，中途无法取消。

    2，如果不设置回调函数，`Promise`抛出的错误，不会反应出来。

    3，当处于 `pending` 状态时，无法得知目前进展到哪一个阶段。

12. #### 手写 Promise 实现

    简版封装promise

    ```js
    //模拟
    function promise () {
      this.msg = '' // 存放value和error
      this.status = 'pending'
      var that = this
      var process = arguments[0]
    
      process (function () {
        that.status = 'fulfilled'
        that.msg = arguments[0]
      }, function () {
        that.status = 'rejected'
        that.msg = arguments[0]
      })
      return this
    }
    //使用
    promise.prototype.then = function () {
      if (this.status === 'fulfilled') {
        arguments[0](this.msg)
      } else if (this.status === 'rejected' && arguments[1]) {
        arguments[1](this.msg)
      }
    }
    ```

    

    Promise 的用法

    ```js
    //创造了一个Promise实例
    const myPromise = new Promise((resolve,reject)=>{
        //....some code
    
        if(/*异步操作成功*/){
           //resolve函数的作用是,将Promise对象的状态从"未完成" 变成 "成功"(即从 pending 变为 resolved),在异步操作成功时调用,并将异步操作结果,作为参数传递出去. 
           resolve(value);
        }else{
          //reject函数的作用是.将Promise对象的状态从"未完成" 变成 "失败"(即从 pending 变为 rejected ),在异步操作失败时调用,并将异步报出的错误,作为参数传递出去.
           reject(error);                  
        }
    })
    //调用
    myPromise().then(res=>{
        //成功，resolved
    },error=>{
        //失败， rejected
    })
    ```

    

13. #### Promise.finally实现

    由于`Promise.then(f,f)` 里面的两个函数有且只有一个被执行，不知道哪个会被执行，所以有了 `.finally()`的出现。Promise.finally 无论成败都会执行的一个函数。

    ```js
    // 终极方法finally finally其实就是一个promise的then方法的别名，在执行then方法之前，先处理callback函数
    Promise.prototype.finally = (callback)=>{
        let P = this.constructor;
        //console.log(P)
        return this.then(
            value => P.resolve(callback()).then(()=> value),
            reason => P.reject(callback()).then(()=> { throw reason })
        )
    }
    //调用与 .then 一样，但回调函数中不接收任何参数
    ```

    

14. #### Generator了解

    > generator是异步操作的方式之一。无法像普通函数一样调用，需要使用 generator对象的 next() 启动。
    >
    > - 第一次使用next() 相当于启动整个 generator 函数的开关
    > - 接下来使用使用一次，相当于触发 `yield` 向后走。

    | 异步操作方式 | 适用场景                                          |
    | ------------ | :------------------------------------------------ |
    | generator    | 带逻辑的异步，简易程度 generator > 回调 > promise |
    | promise      | 没有逻辑依赖的异步，一次性获取很多数据            |
    | 回调         | 处理逻辑异步操作比 promise 好用点，传统方式       |

    基础用法：使用yield ,暂停函数，等待next()触发，几个yield,就有几个 `+1` next()。

    yield 传参(外部调用向内部传参): 在 `next(param)`  中传参，函数中通过赋值 `yield` 接收.

    yield 返回(外部调用接收内部返回):：`yield`返回 `yield`+`返回值`的方式返回，通过赋值 `generator` 对象的 `next()`调用获取返回值。

    ```js
    //声明generator,另一种 function *show(){}
    function* show(){
        alert('1')
        let a = yield;
        console.log('show a ',a)//获取第一个next()传参 [1,2,3]
        let b = yield;
        console.log('show b ',b)//获取第二个next()传参 [4,5,6]
        yield 'yield3'; //第三个next()拿到值
        yield 'yield4'; //第四个 next()拿到值
        alert('2')
    }
    let step = show();
    let obj = { a:[1,2,3], b:[4,5,6]}
    let res1 = step.next();//首次调用相当于启动
    let res2 = step.next(obj.a);//相当于第一个yield
    let res3 = step.next(obj.b);//相当于第二个yield
    let res4 = step.next();//第三个yield
    let res5 = step.next();//第四个yield
    console.log(res1) //{value: "undefined", done: false}
    console.log(res2) //{value: undefined, done: true}
    console.log(res3) //{value: "yield3", done: false}
    console.log(res4) //{value: "yield4", done: false}
    console.log(res5) //{value: undefined, done: true}
    ```

    

15. #### 观察者模式

    又称发布-订阅模式, 举例子说明.
    实现: 发布者管理订阅者队列, 并有新消息推送功能. 订阅者仅关注更新就行

16. ####  构造函数实现原理？

    - `new`创建的实例，可以访问到构造函数里面的属性和方法，以及原型的属性及方法。

    - 直接给 `this` 对象赋值属性和方法， `this` 即指向创建的对象。

    - 没有 `return` 返回值。

      

    ```js
    var Car = function(color){
        this.color = color;
    }
    Car.prototype.start = function(){
    	console.log(this.color + “car start”);
    }
    var car = new Car('black');//正常用法
    car.color;//访问构造函数的属性
    car.start()//访问原型里的属性
    
    //模拟代码
    function create() {
        var obj = new Object();//创建空对象
        Con = [].shift.call(arguments);//获取构造函数，arguments中去除第一个参数。
        obj.__proto__ = Con.prototype;//链接到原型，obj 可以访问到构造函数原型的属性。
        var ret = Con.apply(obj,arguments);//绑定this 实现继承，obj可访问到构造函数的属性和方法
        return ret instanceof Object ? ret : obj;//优先返回构造函数返回的对象
    }
    ```

    

17. #### 节流和防抖

    节流：一定时间内，只让函数触发的第一次生效，后面的不生效，也就是大于设定时间才能执行第二次（节流）。

    举例：滚动条只输出起始位置

    ```js
    window.onload = function(){
        docment.onscroll = throttle(function(){
            console.log("scroll事件触发了" + Date.now())
        },1000)
    }
    function throttle(fn,delay){
        var lastTime = 0;
        return function(){
            var args = arguments;
            var nowTime = Date.now();
            if(nowTime - lastTime > delay ){
               fn.apply(this,args)
                lastTime = nowTime;
            }
        }
    }
    ```

    防抖：一定时间内，只让函数触发的最后一次生效，前面的不生效。

    举例：防止重复点击。狂点按钮，只能让最后一次触发生效。

    ```ht
    <button id='btn'>按钮</button>
    ```

    ```js
    window.onload = function(){
       document.getElementById('btn').onclick = debounce(function(){
           console.log("点击了按钮"，Date.now())
       },1000)
    }
    function debounce(fn,delay){
       var timer = null;
        return function(){
            var that = this;
            var args = arguments;
            clearTimeout(timer);
            timer = setTimeout(function(){
                fn.apply(that,args);
            },delay)
        }
    }
    ```

    

18. #### setTimeout 时间延迟为何不准?

    JS单线程,先执行同步主线程，再执行异步任务队列。

19. #### 事件循环述，宏任务和微任务有什么区别？举例哪个先执行？

    宏任务一般是：包括整体代码script，setTimeout，setInterval。

    微任务：Promise，process.nextTick，都是异步任务。

    1. - 先执行同步主线程，再执行异步任务队列

    2. - 宏任务队列 和微任务队列都有任务的时候，先微任务后宏任务

    ```js
    console.log('同步-0-1')
    setTimeout(function(){
        console.log('异步-s-1')
    });
    new Promise((resolve,reject)=>{
        console.log('同步-0-2')
        resolve(2)
    }).then(data => console.log('异步-p-1'))
    console.log('同步-0-3');
    //同步-0-1
    //同步-0-2
    //同步-0-3
    //异步-p-1
    //异步-s-1
    ```

    先主线程，也就是同步任务。所以先输出 同步log,`同步-0-1,同步-0-2,同步-0-3`

    后异步队列,  异步队列中同时又宏任务和微任务，先微（promise）`异步-p-1`后宏(setTimeout) `异步-s-1`。

20. #### 实现一个sleep函数

    利用伪死循环阻塞主线程实现。因为JS是单线程的，`setTimeout()` 的时间延迟不准确，这种方式更接近真正意义上的`sleep()`。

    ```js
    function sleep(delay) {
       	var start = (new Date()).getTime();
    	while ( (new Date()).getTime() - start < delay ) {
            continue;
        } 
    }
    
    
    function test() {
        console.log('11111',(new Date()).getTime())
        sleep(2000)
        console.log('22222',(new Date()).getTime())
    }
    test()
    ```

    

21. #### js实现instanceof ?

    instanceof 常用来判断数据类型。

    缺点：不能区分 undefined和null。基本类型不是用new 声明的也不能区分。

    优点：对于 new声明的类型可以检测多层继承关系。

    重写后：

    还是不能区分 undefined和null(单独判断)。但其他可以。

    ```js
    function myInstanceof(L, R) {//L 表示左表达式，R 表示右表达式 
        var O = R.prototype;   // 取 R 的显示原型 
        L = L.__proto__;  // 取 L 的隐式原型
        while (true) {    
            if (L === null)      
                 return false;   
            if (O === L)  // 当 O 显式原型 严格等于  L隐式原型 时，返回true
                 return true;   
            L = L.__proto__;  
        }
    }
    //用法
    console.log(myInstanceof([1,2,3],Array)) //true
    ```

    测试(扩展，可不用)

    ```JS
    var bool = true
    var num = 1
    var str = 'abc'
    var und = undefined
    var nul = null
    var arr = [1,2,3]
    var obj = {name:'haoxl',age:18}
    var fun = function(){console.log('I am a function')}
    
    console.log(myInstanceof(arr,Array)) //true
    console.log(myInstanceof(str,Array)) //false
    console.log(myInstanceof(str,String)) //true
    console.log(myInstanceof(fun,Function)) //true
    //判断类
    function Person(){}
    var per = new Person()
    console.log(myInstanceof(per,Person));// true
    function Student(){}
    Student.prototype = new Person()
    var haoxl = new Student()
    console.log(myInstanceof(haoxl,Student)) //true
    console.log(myInstanceof(haoxl,Person)) //true
    
    console.log(myInstanceof(nul,Object)) //报错
    console.log(myInstanceof(und,Object)) //报错
    
    ```

    （扩展）提供一种，终结封装判断数据类型版本

    ```js
    var Type = (function() {
          var type = {};
          var typeArr = ['String', 'Object', 'Number', 'Array','Undefined', 'Function', 'Null', 'Symbol'];
          for (var i = 0; i < typeArr.length; i++) {
              (function(name) {
                  type['Is' + name] = function(obj) {
                      return Object.prototype.toString.call(obj) == '[object ' + name + ']';
                  }
              })(typeArr[i]);
          }
          return type;
    })();
    //用法, Is + 类型()
    console.log(Type.IsFunction(function() {})) //true
    console.log(Type.IsObject({})) //true
    console.log(Type.IsUndefined(undefined)) //true
    ```

22. #### 手写实现 bind ?

23. #### http 状态码 ?

24. #### async 和 await ?

25. #### 算法和数据结构

26. #### 封装JSONP

27. #### ajax 和axios、fetch 的区别？

28. #### 手动实现map(foreach 以及 filter 也类似)

29. #### JS实现 checkbox 全选以及反选?

    ```html
    <body>
        <button id="other">
            反选
        </button>
        <input type="checkbox" id="all" text="全选"/>
    </body>
    ```

    

30. #### JavaScript 中 call()、apply()、bind() 的用法

通用测试代码

```js
var name = '小王',age = 17;
var obj  = {
    name:'小张',
    objAge : this.age,//此处this 指向全局 window
    myFun : function(form,to){
       console.log( this.name + "年龄" + this.age , "来自" + form + "去往" + to);//此处this 指向 obj
    }
}
console.log(obj.objAge)//17
obj.myFun() //小张年龄undefined 来自undefined去往undefined
var db = {
    name : "德玛",
	age: 99
}
```

- **call()、apply()、bind() 都是用来重定义 this 这个对象的.**

```js
obj.myFun.call(db)    // 德玛年龄 99
obj.myFun.apply(db)   // 德玛年龄 99
obj.myFun.bind(db)()   // 德玛年龄 99
```

以上除了 bind 方法后面多了个 () 外 ，结果返回都一致！

由此得出结论，**bind 返回的是一个新的函数**，你必须调用它才会被执行。

- **对比call 、bind 、 apply 传参情况下**

  ```js
  obj.myFun.call(db,'成都','上海');   //德玛年龄99 来自成都去往上海
  obj.myFun.apply(db,['成都','上海']);   //德玛年龄99 来自成都去往上海
  obj.myFun.bind(db,'成都','上海')();   //德玛年龄99 来自成都去往上海
  obj.myFun.bind(db,['成都','上海'])();   //德玛年龄99 来自成都,上海去往undefined
  ```

  相同点：

  ​         call 、bind 、 apply 这三个函数的第一个参数都是 this 的指向对象，直接放进去。

  不同点：

  ​			`call`第二第三第n个参数，用逗号隔开，直接放到后面。`obj.myFun.call(db,'成都',...,'string')`

  ​			`apply` 第二第三第n个参数，必须放一个数组中传进去。`obj.myFun.call(db,['成都',...,'string'])`

  ​			`bind` 除了返回的是函数以外，参数设置和 `call` 一样。

参数不限制类型，亦可以是 函数、object等