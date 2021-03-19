### 通过 eject 命令释放 webpack 文件

react-scripts 是 create-react-app 的一个核心包，一些脚本和工具的默认配置都集成在里面，而 yarn eject 命令执行后会将封装在 create-react-app 中的配置全部反编译到当前项目，这样用户就能完全取得 webpack 文件的控制权。所以，eject 命令存在的意义就是更改 webpack 配置，通过 eject 方法释放的配置文件这个操作是不可逆的，要谨慎

\```js

npm run eject

\```

###  react-app-rewired

  2.1 安装react-app-rewired

 \```js

  npm install react-app-rewired --save-dec

  \```

  2.2 在 package.json 中，将原本的 react-script 改为 react-app-rewired

  \```js

  "scripts": {

​    "start": "react-app-rewired start",

​    "build": "react-app-rewired build",

​    "test": "react-app-rewired test",

​    "eject": "react-app-rewired eject"

  }

  \```

  在根目录下新建config-overrides.js,在config-overrides.js里更改配置项，项目启动的时候会先在config-overrides.js里读数据，对webpack里的默认文件进行整合，最后才会启动。



  要对 webpack 配置，还需要安装 customize-cra 包



  \```js

npm install customize-cra --save-dev

  \```



  customize-cra 利用 react-app-rewired 和 config-overrides.js 文件。通过导入customize-cra 函数并导出包装在我们的 override 函数中的一些函数调用，您可以轻松地修改构成 create-react-app 的基础配置对象（webpack，webpack-dev-server，babel等）。



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

