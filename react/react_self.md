### 什么是虚拟dom

是一个js对象。是一种编程概念。用js对象来描述真实的dom节点。



### 为什么用虚拟dom对象



### react中哪里用到了虚拟dom对象，怎么用的？虚拟dom是和真实的dom作对比的吗



### 什么是JXS

语法糖



React17  不需要引用react，react 16需要？？？？？？

jsx为什么会变转成js对象？？vnode对象是怎么来的？

虚拟dom不一定能提高性能表现？是的





类组件在js里面还是函数



Document.createDocumentFragment();

Document.createTextNode()



### diff算法

深度优先遍历





react不要随便改type值，一旦type值变了，就算其下子节点灭有变化，也不会复用了。



### 为什么用fiber

任务分解，更流程，一个js对象 

### fiber结构







1、虚拟dom是什么？
广义是一个编程概念，表示虚拟的dome结构。 本质就是一个js对象，用这个对象来表示真实的dom的结构和信息，js对象存在于内存中，通过react-dom的render，和真实的dom同步（这个过程叫协调）。
2、为什么要用虚拟dom?
dom操作耗性能，且真实的dom的标签有太多属性我们无需用， 操作真实dom轻易引起重画和重排（回流），耗渲染资源。
js对象处理起来更快， 也更容易diff新旧vdom差异，从而提升性能。
3、babel-loader会预编译JSX为React的什么函数？
  React.CreateElement(type, config, ...children);
4、react渲染过程是咋样？
   （1）通过webpack + babel编译时， 把jsx语法转化成React.createElement(.....)
     (2) react.createElement(type, config, ...children) 根据参数得到一个vdom, 它能完整的描述dom结构和属性
     (3) ReactDom.render(vdom, container) 将vdom 转化为 dom 并插入到Container中。
5、react diff算法的复杂度是多少？
O(n)
6、Fiber节点什么时候可以复用？
       同层级之下的type和key相同的节点
7、React.PureComponent 和 Component 有什么区别？
pureComponent通过prop和state的浅比较（shallowEqual）来实现了shouldComponentUpdate。





区别：花同样的时间去学习一个东西，别人用心了，你总是觉得反正是自学，那就随便学学吧的态度，无所谓的态度，所以你收获的总是比别人少。一个东西你既然去学习了就好好学习，不要觉得什么都无所谓！！！！学了就学会，弄懂。































