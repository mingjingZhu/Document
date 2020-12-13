# JavaScript 引擎



### 一. 了解JavaScript的特点

（1）运行时解释语言。

（2）有自动垃圾回收机制。

（3）弱数据类型。

（5）动态类型(dynamic typing)：运行的时候才确定对象的类型。

（6）JavaScript没有内置的I/O机制。

### 二. 什么是JavaScript引擎

> JavaScript引擎是一个专门处理JavaScript脚本的<font color="blue">虚拟机</font>，一般会附带在网页浏览器之中。

简单地说，JavaScript解析引擎就是能够“读懂”JavaScript代码，并准确地给出代码运行结果的一段程序。比方说，当你写了 let a = 1 + 1; 这样一段代码，JavaScript引擎做的事情就是看懂（解析）这段代码，并且将a的值变为2。

学过编译原理的人都知道，对于静态语言来说（如Java、C++、C），处理上述这些事情的叫**<font color="orange">编译器（Compiler）</font>**，相应地对于JavaScript这样的动态语言则叫**<font color="orange">解释器（Interpreter）</font>**。这两者的区别用一句话来概括就是：**<font color="orange">编译器是将源代码编译为另外一种代码（比如机器码，或者字节码），而解释器是直接解析并将代码运行结果输出</font>**。 比方说，<font color="blue">firebug</font>的console就是一个JavaScript的解释器。

```
附加 
编译型语言：程序在执行之前需要一个专门的编译过程，把程序编译成为机器语言的文件（即exe文件），运行时不需要重新编译，直接用编译后的文件（exe文件）就行了。
优点：执行效率高
缺点：跨平台性差，也就是不能在不同的操作系统之间随意切换。
可执行程序不能跨平台很容易理解，因为不同操作系统对可执行文件的内部结构有着截然不同的要求，彼此之间也不能兼容。不能跨平台是天经地义，能跨平台反而才是奇葩。比如，不能将 Windows 下的可执行程序拿到 Linux 下使用，也不能将 Linux 下的可执行程序拿到 Mac OS 下使用（虽然它们都是类 Unix 系统）。
解释型语言：程序不需要编译，程序在运行的过程中才用解释器编译成机器语言，边编译边执行（没有exe文件）。
优点：跨平台性好
缺点：执行效率低
```

但是，现在很难去界定说，JavaScript引擎它到底算是个解释器还是个编译器，因为，比如像**V8**（Chrome的JS引擎），使用了及时编译技术<font color="blue">  **JIT编译器（Just In Time Compilation）** </font>  它其实为了提高 JS的运行性能，在运行之前会先将JS编译为本地的机器码（native machine code），然后再去执行机器码（这样速度就快很多）。

```
附加 - JavaScript比Java慢的原因
和大多数解释型语言一样，JavaScript运行也比较慢，和Java等静态编译语言相比，究其原因大概有：
- JavaScript变量无类型信息，不能做偏移信息查找，偏移信息共享等编译阶段的优化。
- JavaScript将源码编译为字节码的过程要占用运行时间，而Java的编译则是在运行之前，不占用任何运行时间。
```

这里还要说明的就是，JavaScript引擎本身也是程序，代码编写而成。比如V8就是用C++写的。

### 三. JavaScript引擎与ECMAScript是什么关系？

JavaScript引擎是一段程序，我们写的JavaScript代码也是程序，如何让程序去读懂程序呢？这就需要定义规则。比如，之前提到的*let a = 1 + 1;*，它表示：

- 左边let代表了这是申明（declaration），它申明了a这个变量
- 右边的+表示要将1和1做加法
- 中间的等号表示了这是个赋值语句
- 最后的分号表示这句语句结束了

上述这些就是规则，有了它就等于有了衡量的标准，JavaScript引擎就可以根据这个标准去解析JavaScript代码了。那么这里的 ECMAScript就是定义了这些规则。其中ECMAScript 262这份文档，就是对JavaScript这门语言定义了一整套完整的标准。其中包括：

- var，if，else，break，continue等是JavaScript的关键词
- abstract，int，long等是JavaScript保留词
- 怎么样算是数字、怎么样算是字符串等等
- 定义了操作符（+，-，>，<等）
- 定义了JavaScript的语法
- 定义了对表达式，语句等标准的处理算法，比如遇到**==**该如何处理
- ⋯⋯

**标准**的JavaScript引擎就会根据这套文档去实现，注意这里强调了**标准**，因为也有不按照标准来实现的，比如IE的JS引擎。这也是为什么JavaScript会有兼容性的问题。至于为什么IE的JS引擎不按照标准来实现，就要说到浏览器大战了，这里就不赘述了，自行Google之。

所以，简单的说，ECMAScript定义了语言的标准，JavaScript引擎根据它来实现，这就是两者的关系。

### 四. JavaScript引擎和渲染引擎的关系

网页的工作过程不能缺少的两个引擎，渲染引擎和JavaScript引擎。JavaScript引擎负责执行JavaScript代码，渲染引擎负责渲染网页。JavaScript引擎提供调用接口给渲染引擎，以便让渲染引擎使用JavaScript引擎来处理JavaScript代码并获取结果。JavaScript引擎需要能够访问渲染引擎构建的DOM树，所以JavaScript引擎通常需要提供桥接的接口，渲染引擎根据桥接接口来提供让JavaScript访问DOM的能力。

两种引擎通过桥接接口来构建DOM结构，造成了性能的损失。所以目前为止使用JavaScript操作DOM还是一个非常低效率的事。目前主流的解决方案是使用虚拟DOM的方式。

### 五. JavaScript引擎与浏览器又是什么关系？
简单地说，JavaScript引擎是浏览器的组成部分之一。因为浏览器还要做很多别的事情，比如解析页面、渲染页面、Cookie管理、历史记录 等等。那么，既然是组成部分，因此一般情况下JavaScript引擎都是浏览器开发商自行开发的。比如：IE9的Chakra、Firefox的 TraceMonkey、Chrome的V8等等。

从而也看出，不同浏览器都采用了不同的JavaScript引擎。因此，**我们只能说要深入了解哪个JavaScript引擎**。

```
附加 - 知名JavaScript引擎:
（1）V8(Google)，用C++编写，开放源代码，Google (丹麦)开发 ，也用于Node.js。
（2）JavaScriptCore(Apple)，开放源代码，用于webkit型浏览器，如Safari。
（3）Rhino，由Mozilla基金会管理，开放源代码，完全以Java编写，用于HTMLUnit
（4）SpiderMonkey(Mozilla)，第一款JavaScript引擎，早期用于Netscape Navigator，现时用于Mozilla Firefox。
（5）Chakra (Microsoft)(JScript引擎)，用于Internet Explorer。
（6）Chakra (Microsoft)(JavaScript引擎)，用于Microsoft Edge。
（7）KJS，KDE的ECMAScript／JavaScript引擎，最初由哈里·波顿开发，用于KDE项目的Konqueror网页浏览器中。
```



#### 只可意会不可言传 一 <font color="orange">不需要过分去强调JavaScript引擎到底是什么，了解它究竟做了什么事情就可以了。下面就了解一下JavaScript引擎的执行机制</font>(<font size=3>对于编译器或者解释器究竟是如何看懂代码的，翻出大学编译课的教材就可以了。)</font>



### 六.  JavaScript 引擎的执行机制

重要两点

- JS是单线程语言
- JS的Event Loop是JS 的执行机制。深入了解JS的执行，就等于深入了解JS的Event Loop。

*<font color="green">灵魂三问：JS为什么是单线程的？为什么需要异步？单线程又是如何实现异步的呢？</font>*

**(1) JS为什么是单线程的？**

这跟历史有关系。JavaScript从诞生起就是单线程。原因大概是不想让浏览器变得太复杂，因为多线程需要共享资源、且有可能修改彼此的运行结果，对于一种网页脚本语言来说，这就太复杂了。后来就约定俗成，JavaScript为一种单线程语言。----- 阮一峰。

想象一下，如果浏览器中的JS是多线程的。

场景描述：

那么现在有2个进程，process1 process2，由于是多进程的JS，所以他们对同一个dom，同时进行操作。process1 删除了该dom，而process2 编辑了该dom，同时下达2个矛盾的命令，浏览器究竟该如何执行呢？

这样想，JS为什么被设计成单线程应该就容易理解了吧。

**(2) JS为什么需要异步?**

场景描述：

如果JS中不存在异步，只能自上而下执行，如果上一行解析时间很长，那么下面的代码就会被阻塞。对于用户而言，阻塞就意味着"卡死"，这样就导致了很差的用户体验。

所以，JS中存在异步执行。

**(3) JS单线程又是如何实现异步的呢？**

既然JS是单线程的，只能在一条线程上执行，又是如何实现的异步呢？

是通过的事件循环([event loop](https://www.ruanyifeng.com/blog/2013/10/event_loop.html))，理解了event loop机制，就理解了JS的执行机制。

由于之前已有多人分享过event loop，本次不再赘述，大概描述一下浏览器是如何实现异步的。

如果某个任务很耗时，比如涉及很多I/O（输入/输出）操作，那么线程的运行大概是下面的样子。

![222](https://www.ruanyifeng.com/blogimg/asset/201310/2013102002.png)

上图的绿色部分是程序的运行时间，红色部分是等待时间。可以看到，由于I/O操作很慢，所以这个线程的大部分运行时间都在空等I/O操作的返回结果。这种运行方式称为"同步模式"（synchronous I/O）或"堵塞模式"（blocking I/O）。

如果采用多线程，同时运行多个任务，那很可能就是下面这样。

![23](https://www.ruanyifeng.com/blogimg/asset/201310/2013102003.png)

上图表明，多线程不仅占用多倍的系统资源，也闲置多倍的资源，这显然不合理。

Event Loop就是为了解决这个问题而提出的。[Wikipedia](http://en.wikipedia.org/wiki/Event_loop)这样定义：

> "**Event Loop是一个程序结构，用于等待和发送消息和事件。**（a programming construct that waits for and dispatches events or messages in a program.）"

简单说，就是在程序中设置两个线程：一个负责程序本身的运行，称为"主线程"；另一个负责主线程与其他进程（主要是各种I/O操作）的通信，被称为"Event Loop线程"（可以译为"消息线程"）。

![4](https://www.ruanyifeng.com/blogimg/asset/201310/2013102004.png)

上图主线程的绿色部分，还是表示运行时间，而橙色部分表示空闲时间。每当遇到I/O的时候，主线程就让Event Loop线程去通知相应的I/O程序，然后接着往后运行，所以不存在红色的等待时间。等到I/O程序完成操作，Event Loop线程再把结果返回主线程。主线程就调用事先设定的回调函数，完成整个任务。

可以看到，由于多出了橙色的空闲时间，所以主线程得以运行更多的任务，这就提高了效率。这种运行方式称为"异步模式"（asynchronous I/O）或"非堵塞模式"（non-blocking mode）。

总结：

JS的单线程是指一个浏览器进程中只有一个JS的执行线程，同一时刻内只会有一段代码在执行（你可以使用浏览器的标签式浏览试试看效果，这时打开的多个页面使用的都是同一个JS执行线程，如果其中一个页面在执行一个运算量较大的function时，其他窗口的JS就会停止工作）。
**而异步机制是浏览器的两个或以上常驻线程共同完成的，**例如异步请求是由两个常驻线程：JS执行线程和事件触发线程共同完成的，JS的执行线程发起异步请求（这时浏览器会开一条新的HTTP请求线程来执行请求，这时JS的任务已完成，继续执行线程队列中剩下的其他任务），然后在未来的某一时刻事件触发线程监视到之前的发起的HTTP请求已完成，它就会把完成事件插入到JS执行队列的尾部等待JS处理。



### 七. 深入了解其内部原理的途径有哪些？

个人认为，主要途径有如下几种（依次由浅入深）：

- 看讲JavaScript引擎工作原理的书
  这种方式最方便，不过我个人了解到的这样的书几乎没有，但是[Dmitry A.Soshnikov](http://dmitrysoshnikov.com/)博客上的文章真的是非常的赞，建议直接看英文，实在英文看起来吃力的，可以看译本。
- 看ECMAScript的标准文档
  这种方式相对直接，原汁原味，因为引擎就是根据标准来实现的。目前来说，可以看[第五版](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf)和[第三版](http://www.ecma-international.org/publications/files/ECMA-ST-ARCH/ECMA-262, 3rd edition, December 1999.pdf)，不过要看懂也是不容易的。
- 看JS引擎源代码
  这种方式最直接，当然也最难了。因为还牵涉到了如何实现词法分析器，语法分析器等等更加底层的东西了，而且并非所有的引擎代码都是开源的。





*补充定义：*

**1. 虚拟机 :** 在计算机科学中的体系结构里，是指一种特殊的软件，可以在计算机平台和终端用户之间创建一种环境，而终端用户则是基于这个软件所创建的环境来操作软件。
虚拟机的实现主要分为三类：
\- 系统虚拟机，提供一个可以运行完整操作系统的完整系统平台。典型代表：VirtualBox。
\- 程序虚拟机，运行单个计算机程序设计，这意谓它支持单个进程。典型代表：JavaScript引擎、Java 虚拟机。

\- 操作系统层虚拟化：介于系统和单个程序之间，可以运行多个独立应用程序，但是又不用虚拟完整操作系统。典型代表：Docker。

**2. firebug :** 是firefox下的一个扩展(开源的工具软件)，能够调试所有网站语言，如Html,Css等，但FireBug最吸引人的就是javascript调试功能，使用起来非常方便，而且在各种浏览器下都能使用（IE,Firefox,Opera, Safari）。

**3. JIT编译 :**   （just-in-time compilation）混合了编译器和解释器，在边编译边运行的过程中会将编译过的代码缓存起来，下次运行的时候运行的就是编译后的代码。狭义来说是当某段代码即将第一次被执行时进行编译，因而叫“即时编译”。*JIT编译是动态编译的一种特例*。JIT编译一词后来被*泛化*，时常与动态编译等价；但要注意广义与狭义的JIT编译所指的区别。

---

参考文档：

https://www.cnblogs.com/xinxingyu/p/4928587.html

https://zhuanlan.zhihu.com/p/41396928

https://zhuanlan.zhihu.com/p/84225436

https://www.ruanyifeng.com/blog/2013/10/event_loop.html

