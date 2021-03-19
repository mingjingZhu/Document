## react 配置commitlint

commit格式：<type, 必填>(<scope，可省略>): <subject，必填>

```
type用于说明 commit 的类别
scope用于说明 commit 影响的范围，比如某个模块、某个功能。
subject是 commit 目的的简短描述，不超过50个字符。
```

#### 一、校验commit message

1. 安装commitlint

   ```js
   yarn add @commitlint/config-conventional --dev
   yarn add @commitlint/cli --dev
   ```

2. 添加`commitlint.config.js` 文件

   ```js
   echo "module.exports = {extends: ['@commitlint/config-conventional']}" > commitlint.config.js
   
   module.exports = {
     extends: ["@commitlint/config-conventional"],
     rules: {
       "type-enum": [
         2,
         "always",
         [
           "feat", // 新特性、需求
           "fix",
           "docs",
           "style", // 样式
           "refactor", // 功能重构
           "test", // 测试(单元测试、集成测试)
           "revert",
           "chore", // 依赖库、工具(新增package依赖、工具类等)
           "build"
         ],
       ],
     },
   };
   ```

3. 安装husky

   ```js
   yarn add husky --dev  "husky": "^4.3.8",
   ```

4. 配置 package.json 文件 --- 添加 husky 字段

   ```js
   "husky": {
       "hooks": {
         "commit-msg": "commitlint -e $HUSKY_GIT_PARAMS"
       }
    },
   ```

#### 二、提供规范的commit message   -----   Commitizen 使用

> Commitizen 是一个撰写符合 Commit Message 标准的一款工具。

```
全局安装使用 git cz 来代替 git commit

局部安装使用 npm run commit 脚本来代替 git commit
```

###### 全局安装，此处不重点讲，主看项目级安装

1. 全局安装包

   ```js
   npm install -g commitizen cz-conventional-changelog
   // yarn是怎么写的？yarn global add typescript
   ```

2. 创建 ~/.czrc 文件，写入如何内容

   ```js
   { "path": "cz-conventional-changelog" }
   ```

   or

   ```js
    echo '{ "path": "cz-conventional-changelog" }' > ~/.czrc
   ```

3. 这时就可以全局使用 git cz 命令来代替 git commit 命令了

###### 项目级安装

1. 安装包

   ```js
   yarn add commitizen --dev
   ```

2. 配置package.json文件

   ```js
   {
     "scripts": {
       "commit": "git-cz",
     },
     "config": {
       "commitizen": {
         "path": "node_modules/cz-conventional-changelog"
       }
     }
   }
   ```

3. 这时就可以使用 npm run commit 脚本了

#### 三、生成 Changelog

1. conventional-changelog-cli 就是生成 CHANGELOG 的工具，我们首先来安装一下：

   ```js
   npm install -g conventional-changelog-cli
   ```

2. 通过执行以下命令，则可以生成 CHANGELOG.md 文件：

   ```js
   conventional-changelog -p angular -i CHANGELOG.md -s
   
   # 不会覆盖以前的 Change log，只会在 CHANGELOG.md 的头部加上自从上次发布以来的变动
   $ conventional-changelog -p angular -i CHANGELOG.md -s -p 
   
   # 生成所有发布的 Change log
   $ conventional-changelog -p angular -i CHANGELOG.md -w -r 0
   ```

   我们也可以将该命令配置到`scripts`中，就可以通过执行`npm run changelog`命令来生成 CHANGELOG 了:

   ```js
   {
     "scripts": {
       "changelog": "conventional-changelog -p angular -i CHANGELOG.md -s"
     }
   }
   ```

   如若你不需要手动修改CHANGELOG.md文档，可以自动提交CHANGELOG.md文档：

   ```js
   conventional-changelog -p angular -i CHANGELOG.md -s && git commit -m "docs: 更改CHANGELOG.md"
   ```

   