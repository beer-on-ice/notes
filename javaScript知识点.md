# javaScript #

## 面试题整理：

###  Js相等比较
![](https://i.imgur.com/eqJKzRd.png)

### Q1: 运算符
-	逗号运算符,的作用是对每个表达式求值,并返回最后一个表达式的值.


### Q2: 重复声明两个函数会怎样？

#### A2: 重复申明两个函数跟重复生命两个变量本质上是一样的，都是以最后一次声明为准。

### Q3： 原型链、作用域链
- 案例一：
- 
    function Foo() {
	    getName = function () { 
	    	console.log('1');
	    };
	    return this;
    }
    Foo.getName = function () {
    	console.log('2');
    };
    Foo.prototype.getName = function () { 
    	console.log('3');
    };
    var getName = function () { 
    	console.log('4');
    };
    function getName() { 
    	console.log(5);
    }
    
    Foo.getName();  
    getName();	
    Foo().getName(); 
    getName();  
    new Foo.getName(); 
    new Foo().getName();   
    new new Foo().getName();		

#### A3：
###### JavaScript的操作符优先级,从高到低排序![](https://i.imgur.com/JHbKpqp.png)
###### 运算符优先级![](https://i.imgur.com/8ANDBE4.png)


	-	注意声明前置
    function Foo() {
    	//在函数内部声明的getName变量，前面是不带有var、let,const的，声明的getName是在全局范围内(也是就window)。也就是个全局变量
	    getName = function () {
	    	console.log('1');
	    };
	    return this;
    }
    // 为函数添加属性getName,其类型是Function，所以这里也可以看出来，Function也是一种Object
    Foo.getName = function () {
    console.log('2');
    };
    // 为Foo的原型添加方法getName
    Foo.prototype.getName = function () {
    console.log('3');
    };
    
    // var声明的变量和函数声明function都会被提升，但是函数声明的提升的级别是比var要高的
    
    var getName = function () {
    console.log('4');
    };
    function getName() {
    console.log(5);
    }
    
    Foo.getName();  // => 2 函数Foo本身并没有执行，执行的是函数的属性getName，当然输出的是：2.
    
    getName();	//   => 4在全局范围内执行了getName() ，，，实际顺序是 var getName | function getName(){} | getname = function(){}
    
    Foo().getName();  // =>  ()与.优先级相同，所以从左至右执行。首先运行Foo(), 全局 的getName被覆盖成输出console.log('1'),并且返回的this此时代表的是window。随后相当于执行的window.getName(),那么输出的实际就是1(被覆盖)。
    
    getName();   // => 1 同上
    
    new Foo.getName(); // =>2   . 操作符要比 new 优先级要高，所以实际执行的是new (Foo.getName)()
    
    new Foo().getName(); // =>3  首先看运算符优先级括号高于new。实际执行为(new Foo()).getName()。遂先执行Foo函数。new Foo()返回了新生成的对象，该对象没有getName()方法，所以在prototype中找到了getName()方法。所以输出的是3。 // 具体可看运算符优先级
    
    new new Foo().getName(); // => 3  第一步划分为：new (new Foo().getName)(); 第二步划分为：new ((new Foo()).getName)();
    

- 案例二：
-
    var a = 1;
    
    var a = 1;
    function fn(){
      console.log(a);//第一个输出值---->  undefined
      var a = 5;
      console.log(a);//第二个输出值---->  5  
      a++;
      var a;
      fn3();
      fn2();
      console.log(a);   //第五个输出值  ---->  20
    
      function fn2(){
    console.log(a); //第四个输出值  ---->   6
    a = 20;
      }
    }
    
    function fn3(){
      console.log(a)   //第三个输出值  ---->  1
      a = 200;
    }
    
    fn();
    console.log(a);   //第六个输出值---->  200
>  1、作用域在全局，调用全局变量 a，但由于函数内部变量重新声明，且根据变量声明提前但赋值不会提前，所以输出 undefined
2、作用域在函数 fn 内，调用 fn 内变量 a，此时函数内变量声明且赋值 var a = 5，所以输出为 5
3、作用域在全局，此时调用全局变量 a， 所以输出为 1
4、作用域在 fn 内，调用 fn 内变量 a，此时 fn 内由 a++ 计算后，a = 6，所以输出为 6
5、作用域在 fn 内，调用 fn 内变量 a，此时由于 fn2 函数执行，fn 内变量 a 重新赋值为 a = 20 ，所以输出为 20
6、作用域在全局，调用全局变量 a，此时由于 fn3 函数执行，全局变量 a 重新赋值为 a = 200 ，所以输出为 200

###  Q4:js等号及隐式类型转换等
    console.log( +0 ==  -0 ) // =>true
    
    console.log( +0 === -0 ) // => true
    
    console.log( Object.is(+0, -0) ) // => false   // Object.is()方法确定两个值是否相同。
    
    console.log( "1" == 1 ) // => true
    
    console.log( "1" === 1 ) // => false
    
    console.log( Object.is("1", 1) ) // => false
    
    console.log( new String("foo") == new String("foo") )  //   => false
    
    console.log( new String("foo") === new String("foo") ) // => false
    
    console.log( Object.is(new String("foo") , new String("foo") ) ) // => false
    
    console.log( new String("foo") == "foo" ) // => true
    console.log( new String("foo") === "foo" ) // => false
    console.log( Object.is(new String("foo") , "foo" ) ) // => false
    
    const a = {}
    const b = a
    console.log( a == b  ) // => true
    console.log( a === b  ) // => true
    console.log( Object.is(a, b) ) // => true

    console.log( NaN == NaN ) // => false
    console.log( NaN === NaN ) // => false
    console.log( Object.is(NaN, NaN) ) // => true

#### A4:
可参考![](https://i.imgur.com/oMfY5zJ.png)

### Q5:如何取得页面用到哪几种标签？
#### A5:
-	`document.all`能取得当前页面所有的`element`，判断`nodeType===1`就是`element`了，取`nodeName`就是标签名称，遍历做个类别统计就可以
-	`document.getElementsByTagName('*')`， 类似的还有jq`$("*")`

### Q6： 递归拆分
-	写一个函数，列出一个整数所有的分解类型，比如对于数字4，可以做拆分得到下列字符串1111|112|121|13|211|22|31|4
#### A6: 
-	`Array.from() `方法从一个类似数组或可迭代的对象中创建一个新的数组实例。
-
    function f() {
	    // Array.from() 方法从一个类似数组或可迭代的对象中创建一个新的数组实例。
	    var arr = Array.from(arguments);
	    // console.log(arr);
	    let before = arr.slice(0, arr.length - 1);
	    // console.log(before);
	    let n = arr[arr.length - 1];
	    // console.log(n);
	    for (let i = 1; i < n; i++) {
	    f(...before, i, n - i);
	    }
	    console.log(arr);
    }

### Q7: this指向
#### A7: 
-	this绑定有四种情况；
	-	默认绑定：函数独立调用时，this默认绑定到window
	-	隐式绑定：看函数调用栈，上一个的栈点就是this，console.trace()可查看函数的调用关系
	-	显式绑定：如call,apply,bind，this指向绑定对象
	-	new绑定：this指向new出来的对象

### Q8: 从 URL 输入到页面展现发生了什么?
#### A8： 
[http://blog.csdn.net/tangxiaolang101/article/details/54670218](http://blog.csdn.net/tangxiaolang101/article/details/54670218)

### Q9: 深度克隆
#### A9: 
    function clone(Obj) {
	    var buf;
	    //instanceof判断一个对象是否为某一数据类型，或一个变量是否为一个对象的实例;返回boolean类型
	    if (Obj instanceof Array) {
	    buf = []; //创建一个空的数组
	    var i = Obj.length;
	    while (i--) {
	    buf[i] = clone(Obj[i]);
	    }
	    return buf;
	    } else if (Obj instanceof Object) {
	    buf = {}; //创建一个空对象
	    for (var k in Obj) { //为这个对象添加新的属性
	    buf[k] = clone(Obj[k]);
	    }
	    return buf;
	    } else {
	    return Obj;
	    }
    }

### Q10: 数组去重
#### A10:
-	方法一：

	1. 构建一个新的数组存放结果
	2. for循环中每次从原数组中取出一个元素，用这个元素循环与结果数组对比
	3. 若结果数组中没有该元素，则存到结果数组中
-	方法二：

	1.先将原数组进行排序
	2.检查原数组中的第i个元素 与 结果数组中的最后一个元素是否相同，因为已经排序，所以重复元素会在相邻位置
	3.如果不相同，则将该元素存入结果数组中
-	方法三：

	1.创建一个新的数组存放结果
	2.创建一个空对象
	3.for循环时，每次取出一个元素与对象进行对比，如果这个元素不重复，则把它存放到结果数组中，同时把这个元素的内容作为对象的一个属性，并赋值为1，存入到第2步建立的对象中。
	说明：至于如何对比，就是每次从原数组中取出一个元素，然后到对象中去访问这个属性，如果能访问到值，则说明重复。

[https://github.com/beer-on-ice/Example/tree/master/js/%E6%95%B0%E7%BB%84%E5%8E%BB%E9%87%8D](https://github.com/beer-on-ice/Example/tree/master/js/%E6%95%B0%E7%BB%84%E5%8E%BB%E9%87%8D)

### 原生DOM操作和事件相关

- 如需替换 HTML DOM 中的元素，请使用` replaceChild(newnode,oldnode) `方法
- 从父元素中删除子元素 `parent.removeChild(child)`;
- `insertBefore(newItem,existingItem)` 在指定的已有子节点之前插入新的子节点
- `appendChild(newListItem`向元素添加新的子节点，作为最后一个子节点
document.documentElement - 全部文档
document.body - 文档的主体


http://www.w3school.com.cn/jsref/dom_obj_all.asp

- JS事件：target与currentTarget区别

target在事件流的目标阶段；currentTarget在事件流的捕获，目标及冒泡阶段。只有当事件流处在目标阶段的时候，两个的指向才是一样的， 而当处于捕获和冒泡阶段的时候，target指向被单击的对象而currentTarget指向当前事件活动的对象（一般为父级）。

#### 事件模型

事件捕捉阶段：事件开始由顶层对象触发，然后逐级向下传播，直到目标的元素；
处于目标阶段：处在绑定事件的元素上；
事件冒泡阶段：事件由具体的元素先接收，然后逐级向上传播，直到不具体的元素；
- 阻止 冒泡／捕获 `event.stopPropagation()`和IE的`event.cancelBubble=true`

- DOM事件绑定
1.绑定事件监听函数：addEventListener和attchEvent
2.在JavaScript代码中绑定：获取DOM元素 `dom.onlick = fn`
3.在DOM元素中直接绑定：`<div onclick = 'fn()'>`

DOM事件流包括三个阶段：事件捕获阶段、处于目标阶段、事件冒泡阶段。首先发生的事件捕获，为截获事件提供机会。然后是实际的目标接受事件。最后一个阶段是时间冒泡阶段，可以在这个阶段对事件做出响应。

#### 事件委托

因为事件具有冒泡机制，因此我们可以利用冒泡的原理，把事件加到父级上，触发执行效果。这样做的好处当然就是提高性能了

最重要的是通过`event.target.nodeName`判断子元素

### apply, call和bind有什么区别?

参考答案：三者都可以把一个函数应用到其他对象上，call、apply是修改函数的作用域（修改this指向），并且立即执行，而bind是返回了一个新的函数，不是立即执行．apply和call的区别是apply接受数组作为参数，而call是接受逗号分隔的无限多个参数列表，