## JS中的apply和call方法

> call()方法的作用和 apply() 方法类似，区别就是`call()`方法接受的是**参数列表**，而`apply()`方法接受的是**一个参数数组**。

<font color="blue">**apply的定义**</font>

#### 一、定义

<font color="green">语法</font>

> func.apply(thisArg, [argsArray])

<font color="green">参数</font>

**thisArg**

必选的。在 func 函数运行时使用的 this 值。请注意，this可能不是该方法看到的实际值：如果这个函数处于非严格模式下，则指定为 null 或 undefined 时会自动替换为指向全局对象，原始值会被包装。

**argsArray**

可选的。一个数组或者类数组对象，其中的数组元素将作为单独的参数传给 func 函数。如果该参数的值为 null 或  undefined，则表示不需要传入任何参数。从ECMAScript 5 开始可以使用类数组对象。 

<font color="green">返回值</font>

调用有指定this值和参数的函数的结果。

<font color="green">描述</font>

apply方法主要做了两件事情：（1）修改了this指向 （2）执行了函数func

调用一个对象的一个方法，以另一个对象替换当前对象。 

<font color="blue">**call的定义**</font>

语法：obj1.call(obj2[,param1,param2,...])
定义：用obj2对象来代替obj1，调用obj1的方法。即将obj1应用到obj2上。
说明：call 方法可以用来代替另一个对象调用一个方法。call 方法可将一个函数的对象上下文从初始的上下文改变为由 obj2 指定的新对象。 如果没有提供 obj2参数，那么 Global 对象被用作 obj2。 

#### 二、原理

```js
Function.prototype.imitateCall = function (context) {
    // 赋值作用域参数，如果没有则默认为 window，即访问全局作用域对象
    context = context || window    
    // 绑定调用函数（.call之前的方法即this，前面提到过调用call方法会调用一遍自身，所以这里要存下来）
    context.invokFn = this    
    // 截取作用域对象参数后面的参数
    let args = [...arguments].slice(1)
    // 执行调用函数，记录拿取返回值
    let result = context.invokFn(...args)
    // 销毁调用函数，以免作用域污染
    Reflect.deleteProperty(context, 'invokFn')
    return result
}
```

#### 三、用法总结

1. 改变函数体内部 this 的指向

   可以通过call()和apply()方法间接调用函数。任何函数可以作为任何对象的方法调用。

   我们应该知道一个基础概念，调用的上下文。犀牛书对函数的解析中有描述到：除了实参之外，每次调用还会拥有另一个值——本次调用的上下文——这就是this关键字的值。

   call和apply方法允许显式指定调用所需的this值。也就是说，用call和apply方法间接调用函数时，就是把函数作为显式指定的对象的方法调用。此时，函数内部的this会指向调用对象。

2. 实现继承

   既然call和apply可以实现函数的间接调用并改变被调用的函数this指向。那么就可以妙用this关键字实现继承。如图：在执行FriendA.call(this,"lucy");之前，输出的this 不包含任何属性和方法，之后则继承了FriendA的属性和方法

```js
function FriendA (name) {
    this.name = name;
    this.say = function() {
        console.log("hello");
    }
}

function FriendB () {
    console.log(this);
    FriendA.call(this, "lucy");
    console.log(this);
}

const friend = new FriendB();
```

**FriendB继承了FriendA，FriendA.call(this, "lucy"); 的意思就是使用FriendB对象代替FriendA对象，那么FriendB中不就有FriendA的所有属性和方法了吗，friend对象就能够直接调用FriendA的方法以及属性了。**

*思考20s：如何实现多重继承*

```js
function Test1 () {
    this.reduce = function (a, b) {
        console.log(a - b);
    }
}
function Test2 () {
    this.add = function (a, b) {
        console.log(a + b);
    }
}
function Test3 () {
    Test1.call(this);
    Test2.call(this);
}
const test = new Test3();
test.reduce(3, 2);
test.add(3, 2);
```

#### 四、使用实例

1. 检验数据类型

   ```js
   function type(obj) {
       var regexp = /\s(\w+)\]/;
       var result =  regexp.exec(Object.prototype.toString.call(obj))[1];
       return result;
   };
   
   console.log(type([123]));//Array
   console.log(type('123'));//String
   console.log(type(123));//Number
   console.log(type(null));//Null
   console.log(type(undefined));//Undefined
   ```

2. 数组最大/小值

   因为Math.max/Math.min 里面的参数不支持Math.max([param1,param2]) 也就是数组。但是它支持Math.max(param1,param2,param3…)，所以可以通过apply和call去解决。

   这块在调用的时候第一个参数给了一个null,这个是因为没有对象去调用这个方法,我只需要用这个方法帮我运算,得到返回的结果就行,.所以直接传递了一个null过去。

   ```js
   var arr = [11, 1, 0, 2, 3, 5];
   // 取最大
   var max1 = Math.max.call(null, ...arr);
   var max2 = Math.max.apply(null, arr);
   // 取最小
   var min1 = Math.min.call(null, ...arr);
   var min2 = Math.min.apply(null, arr);
   
   console.log(max1); //11
   console.log(max2); //11
   console.log(min1); //0
   console.log(min2); //0
   ```

3. Array.prototype.push 可以实现两个数组合并

   push方法没有提供push一个数组,但是它提供了push(param1,param,…paramN) 所以同样也可以通过apply/call来装换一下这个数组,即

   ```js
   const arr1 = [1,2,3];
   const arr2 = [4,5,6];
   Array.prototype.push.apply(arr1, arr2);
   console.log(arr1);
   ```

   

----

参考文档

1. https://blog.csdn.net/mafan121/article/details/52922149

2. https://www.cnblogs.com/wdlhao/p/5614522.html