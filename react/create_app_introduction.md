### 通过 eject 命令释放 webpack 文件

react-scripts 是 create-react-app 的一个核心包，一些脚本和工具的默认配置都集成在里面，而 yarn eject 命令执行后会将封装在 create-react-app 中的配置全部反编译到当前项目，这样用户就能完全取得 webpack 文件的控制权。所以，eject 命令存在的意义就是更改 webpack 配置，通过 eject 方法释放的配置文件这个操作是不可逆的，要谨慎

\```js

npm run eject

\```

### react 项目添加ts支持

1. 全局安装ts，便于使用  `tsc`  命令行工具。

   ```js
   npm i -g typescript
   ```

2. 创建tsconfig.json。在需要添加tsconfig.json文件的项目目录中执行：

   ```js
   tsc --init
   ```

3. 安装开发环境依赖。

   ```js
   npm i --save-dev \
   typescript \
   ts-loader \
   @types/react @types/react-dom
   ```

   其中，`ts-loader`是typescript的webpack loader。
    `@types/react`和`@types/react-dom`，是`react`和`react-dom`的[Declaration Files](https://link.jianshu.com?t=https://www.typescriptlang.org/docs/handbook/declaration-files/introduction.html)。



#### react中使用scss

> css modul 相关介绍
>
> 是什么？在项目构建过程中对css类名选择器限定作用域的一种方法
>
> 原理：在构建时可以为每个class生成一个独一无二的哈希字符串类名，实现局部性原理。
>
> 怎么用？配合webpack一起使用，在webpack.config.js文件中进行配置。

1. 使用create-react-app安装完项目之后。执行npm install sass-loader node-sass --save-dev 安装sass这两个相关的插件，相应的webpack脚手架已经配置好了。(注意node的版本)。

2. 上面完成之后react项目就可以支持scss文件的import了，但是不支持`CSS Modules`  import ss from "./index.scss"这种写法。下面介绍css modules使用方法：

   2.1 webpack.config.js   test: /\.(scss|sass)$/  中添加 modules:``true``, 详情下面代码：

   ```js
   {
     test: /\.(scss|sass)$/,
       exclude: sassModuleRegex,
         use: getStyleLoaders(
           {
             importLoaders: 3,
             modules: true, // 是我哦
             sourceMap: isEnvProduction
             ? shouldUseSourceMap
             : isEnvDevelopment,
           },
           'sass-loader'
         ),
           sideEffects: true,
   },
   ```

3. 此时可以使用css modules了，但是检查页面元素的时候，生成的类名看不懂。可以配置一下 localIdentName。

   ```js
   {
     test: /\.(scss|sass)$/,
       exclude: sassModuleRegex,
         use: getStyleLoaders(
           {
             importLoaders: 3,
             sourceMap: isEnvProduction
             ? shouldUseSourceMap
             : isEnvDevelopment,
             modules: {
               localIdentName: "[local]_[hash:base64:5]",
             }
           },
           'sass-loader'
        	),
   }
   ```

