1. 如果想往自己公司私服上发布npm包，只需要将npm的源设置为公司npm的源即可

2. 发布包：

   2.1 登录  npm login

   2.2 在项目根目录下运行`$ npm publish`发布新包。

3. 更新包：

   ```js
   npm version patch
   npm publish
   ```

#### npm各种版本简介

首先：如果后缀 -alpha , 那么说明这个文件目前的这个版本是 内测版本，

    看到内测版本就应该知道，这个版本是有问题的，存在不稳定因素；


其次：如果后缀-beta ,那么说明这个文件目前的这个版本是  公测版本，

    相对于内测又完善了一步，但是依旧是存在不稳定因素的；


最后： 如果后缀的是 -rc ，这个代表的是发布正式版本前的预览版本，和正式的版本很接近了；

#### npm版本号的升级

1. 直接修改package.json中的"version": "1.0.6-beta.0"

#### npm的tag

我们可以通过 `npm dist-tag ls 包名` 来查看一个包的tag，一般来说我们至少会有三种类型的标签

- latest：最后版本，npm install的就是这个
- beta：测试版本，一般内测使用，需要指定版本号install，例如3.1.0-beta.0
- next: 先行版本，npm install foo@next安装，例如3.0.2-alpha.0

如果我们需要发布一个测试版本，在测试版本中 的package.json改成這樣 "version": "1.0.6-beta.0",然後提交測試版本

在发布的时候需要执行

```
npm publish --tag beta
```

如何安装Beta版本

```ks
npm install — save 包名@beta.
```



如果你直接执行`npm publish`，那么即使你的版本号是`-beta.n`，默认会打上`latest`的标签，别人install的时候也会下载到。这个时候需要我们只要改一下tag：

```
// 不小心发错了
latest: 1.0.1-beta.0
// 将1.0.1-beta.0设置为beta
npm dist-tag add my-package@1.0.1-beta.0 beta
npm dist-tag add my-package@1.0.0 latest
复制代码
```

这样就万事大吉了。





#### yarn unlink

- yarn link 前 需要保证你的nodelmodues 或者 lib.js 最新，就说要build一下，在执行yarn link。

- 运行完`yarn unlink [package]`. 您将需要运行 `yarn` 或者 `yarn install` 来重新安装已链接的软件包。

- 先执行`yarn unlink` 再执行`yarn unlink [package]`
- link 的本质就是软链接，这样可以让我们快速使用本地正在开发的其它包。
- 当我们在`npm install -g`的时候，其实是将相关文件安装在`/usr/local/lib/node_modules`目录下，同时在全局命令`/usr/local/bin`目录下会有一个映射脚本，将其指向/usr/local/lib下的真实文件。这么做的好处是，可以在保证只有一份可执行文件的前提下，给命令取别名。

判断link成功：



**### 版本管理**

开发 or 测试: yarn link

预备上线前: 走非正式版本, 比如: 1.6.34.alpha.0

\```

/**

 \* crement alpha version

 \* before v1.0.1-alpha.0

 \* after v1.0.1-alpha.1

*/

$ npm version prerelease --preid=alpha



/**

 \* crement prepatch alpha version

 \* before v1.0.1-alpha.1

 \* after v1.0.2-alpha.0

*/ 

$ npm version prepatch --preid=alpha



$ npm publish --tag alpha



\```

正式上线: 走正式版本号



\- 完成开发后，版本发布 

\```

$ nrm use ez  

$ npm version patch // minor | major  

$ npm publish  

\```









**修改版本号**
`npm version major` : 主版本号加 1，其余版本号归 0。
`npm version minor` : 次版本号加 1，修订号归 0。
`npm version patch` : 修订号加 1。
`npm version 版本号` : 设置版本号为指定的版本号
`npm version prerelease` : 先行版本号增加1
`npm version prerelease --preid=<prerelease-id>` : 指定先行版本的名字

```js
// 假定现在的版本号是1.1.1
npm version major  // 2.0.0
npm version minor  // 1.2.0
npm version patch  // 1.1.2
npm version prerelease // 1.1.2-0
npm version prerelease --preid=alpha // 1.1.2-alpha.0
npm version 4.1.2  // 4.1.2
```









major：主版本号

minor：次版本号

patch：补丁号

premajor：预备主版本

prepatch：预备次版本

prerelease：预发布版本

我的package.jsond的**当前version为6.0.0**，依次输入下面的命令，package的version会变更为提升后的版本号：

```
C:\Users\Administrator\Desktop\work\stage-view (master) (stage-view@6.0.0)
npm version preminor
v6.1.0-0
C:\Users\Administrator\Desktop\work\stage-view (master) (stage-view@6.1.0-0)
npm version minor
v6.1.0
C:\Users\Administrator\Desktop\work\stage-view (master) (stage-view@6.1.0)
npm version prepatch
v6.1.1-0
C:\Users\Administrator\Desktop\work\stage-view (master) (stage-view@6.1.1-0)
npm version patch
v6.1.1
C:\Users\Administrator\Desktop\work\stage-view (master) (stage-view@6.1.1)
npm version prerelease
v6.1.2-0
C:\Users\Administrator\Desktop\work\stage-view (master) (stage-view@6.1.2-0)
npm version premajor
v7.0.0-0
C:\Users\Administrator\Desktop\work\stage-view (master) (stage-view@7.0.0-0)
npm version major
v7.0.0
```



### tag

`npm` 中的 `tag` 类似于 `git` 中的 `branch` ，发布者可以在指定的 `tag` 上进行发版，使用者可以选择指定 `tag` 来安装，默认的`tag`是 `latest`。这对于我们日常开发非常有用，很多时候我们想要发布版本来进行验证功能，但是又不想影响正在使用的人，我们就可以利用tag和先行版本来进行发包。

```js
npm publish --tag alpha  // 发版到名为alpha的tag上
npm i <package>@<tag>    // 从指定tag上安装包
```





npm unpublish z-tool@1.0.0 删除某个版本

npm unpublish的推荐替代命令：npm deprecate <pkg>[@<version>] <message>

使用这个命令，并不会在社区里撤销你已有的包，但会在任何人尝试安装这个包的时候得到警告
例如：npm deprecate z-tool '这个包我已经不再维护了哟～'

















