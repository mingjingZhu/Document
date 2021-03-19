### node执行ts文件

1. 通过ts-node执行

   首先需要安装`typescript`，`ts-node`

   ```js
   npm install typescript -g
   npm install ts-node -g
   ```

   Tsconfig.json示例

   ```js
   {
     "compilerOptions": {
       "module": "commonjs",
       "esModuleInterop": true,
       "target": "es2017",
       "noImplicitAny": true,
       "moduleResolution": "node",
       "sourceMap": true,
       "outDir": "dist",
       "baseUrl": ".",
       "paths": {
         "*": [
           "node_modules/*",
           "src/types/*"
         ]
       },
       "lib": [
         "es2017"
       ],
       "emitDecoratorMetadata": true,
       "experimentalDecorators": true,
       "skipLibCheck": true
     },
     "include": [
       "src/**/*"
     ],
     "experimentalDecorators": true
   }
   ```

   如果没有配置好`tsconfig.json`或者其他安装包，那么运行ts文件 上面代码会出现如下报错：

   ```js
   /usr/local/lib/node_modules/ts-node/src/index.ts:226
       return new TSError(diagnosticText, diagnosticCodes)
              ^
   TSError: ⨯ Unable to compile TypeScript:
   1.ts:2:13 - error TS2580: Cannot find name 'process'. Do you need to install type definitions for node? Try `npm i @types/node`.
   
   2 console.log(process.argv)
   ```

   根据上面的提示，需要安装`types/node`，所以具体步骤如下：

   ```js
   npm init -y
   npm install @types/node
   ```

   再次运行发现已经ok了，说明看控制台报错很重要，会提示你安装一些文件。

2. 项目使用ts

想要获取`tsconfig.json`文件，可以在项目根目录下使用`tsc --init`，就会自动建立好一份`.tsconfig.json`。