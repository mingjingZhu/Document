### JSX

1. what：语法糖。React使用JSX来替代常规的JavaScript。JSX是一个看起来很像XML的JavaScript语法扩展。

2. why：

​	开发效率：使用JSX编写模板简单快速。

​	执行效率：JSX编译为JavaScript代码后进行了优化，执行更快。

​	类型安全：在编译过程中就能发现错误。

3.  React16原理：babel-loader会预编译JSX为React.createElement(...)
4. React17原理：React17中的JXS转换**不会将JSX转换为React.createElement**，而是自动从React的package中引入新的入口函数并调用。另外此次升级不会改变JSX语法，旧的JSX转换也将继续工作。



### react virtual DOM 

What：用js对象表示DOM 信息和结构，当状态变更的时候，重新渲染这个js的对象结构。这个js对象称为virtual dom；

why：DOM操作很慢，轻微的dom操作都可能导致页面重排，非常耗性能。相对于DOM对象，js对象处理起来更快，而且更简单。通过diff算法对比新旧vdom之间的差异，可以批量的、最小化的执行dom操作，从而提高性能。

where：React中用JSX语法描述视图，~~通过babel-loader转译后它们变为React.createElement(...)形式~~，该函数将生成vdom来描述真实dom。将来如果状态变化，vdom将作出相应变化，在通过diff算法对比新老vdom区别从而做出最终dom操作。

### reconciliation协调

设计动力：

### react diff算法

Q: diff算法是和react绑定还是独立个体？

A： 有传统diff算法，今天我们看的是react diff算法。

传统 diff 算法的复杂度为 O(n^3)，显然这是无法满足性能要求的。**React 通过制定大胆的策略，将 O(n^3) 复杂度的问题转换成 O(n) 复杂度的问题**。

### diff策略

1. 同级比较，Web UI中DOM节点跨层级的移动操作特别少，可以忽略不计。
2. 拥有不同类型的两个组件将会生成不同的树形结构。
3. 开发者可以通过`key`  prop 来暗示那鞋子元素在不同的渲染下能保持稳定。

### diff 过程

#### 对比两个虚拟dom时会有三种操作：删除、替换、更新



### 什么是“React Fiber”?

Fiber是React16中新的协调引擎。它的主要目的是使virtual DOM可以进行增量式渲染。

#### Q: Fiber节点什么时候可以复用？

A: 同层级之下的type和key相同的节点

### 总结

1. React17中，React会自动替换JSX为js对象。
2. JS对象即vdom，它能够完整描述dom结构。
3. ReactDOM.render(vdom,container)可以将vdom转换为dom并追加到container中，实际上转换过程需要经过一个diff过程。