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

