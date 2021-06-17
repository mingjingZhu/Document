## Promise.resolve()与new Promise(r => r(v))

### 一、Promise.resolve()介绍







### resolve()本质作用

- resolve()是用来表示promise的状态为fullfilled，相当于只是定义了一个有状态的Promise，但是并没有调用它；
- promise调用then的前提是promise的状态为fullfilled；
- 只有promise调用then的时候，then里面的函数才会被推入微任务中







Promise.resolve()和Promise.reject()常用来生成已经被决议为失败或者成功的promise案例

Promise.reject()简单一些,不管传给它什么值,它决议为失败后就会直接把这个值传递过来





---------

参考文档：

1. https://blog.csdn.net/my_new_way/article/details/104838192

