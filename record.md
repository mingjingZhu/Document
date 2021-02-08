### 1. 如何让Node.js支持ES6的语法

因为nodejs只支持了部分ES6语法，有的ES6语法还不支持，而import、export等就是不支持的。可以升级nodejs的版本，可以支持更多ES6语法。但是升级只能解决nodejs支持的语法部分，下面介绍不用升级也可以解决的几种方法。

#### 方法一：ES6语法修改为ES5语法

例如改import为require。

#### 方法二：安装Babel-cli插件，将ES6转换为ES5;

```js
npm i --save-dev babel-preset-es2015
```

新建一个.babelrc文件，配置内容如下

```js
{
    "presets": ["es2015"],
    "plugins": []
}
```

在node的入口文件加入

