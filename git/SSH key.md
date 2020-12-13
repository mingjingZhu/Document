### 一、如何配置SSH key

1. 步骤一：查看自己是否已经创建了SSH key

```
cd ~/.ssh
ls
```

这两个命令就是检查是否已经存在 id_rsa.pub 或 id_dsa.pub 文件，如果文件已经存在，并且就是你想使用的SSH key，那么跳过步骤2，直接进入步骤3。如果文件已经存在，但是你是想在本地再添加一个SSH key，那么继续步骤2.

2. 步骤二：生成秘钥

```
ssh-keygen -f ~/.ssh/jenkins
```

-t 指定密钥类型，默认是 rsa ，可以省略。
-C 设置注释文字，比如邮箱。（一定要是关联你 GitHub 的注册邮箱，也就是用户名）
-f 指定密钥文件存储文件名。（一般使用默认文件名rsa，会生成 id_rsa 和 id_rsa.pub 两个秘钥文件。因为我此处是要添加，所以指定一个文件名）

接着又会提示你输入两次密码（该密码是你 push 文件的时候要输入的密码，而不是 GitHub 管理者的密码），当然，你也可以不输入密码，直接按回车。那么push的时候就不需要输入密码，直接提交到GitHub 上了。

之后会显示如下提示，表示 SSH key 已经创建成功。

![image-20201213161432178](git.assets/image-20201213161432178.png)

3. 步骤三：添加 SSH key 到 GitHub 上去

确保启用 SSH 代理：

```
eval "$(ssh-agent -s)"
```

为 SSH key 启用 SSH 代理

```
ssh-add ~/.ssh/github
```

如果你使用一个现有的SSH密钥，您需要与您现有的私钥文件名命令取代 github。（当然使用旧密钥就不需要执行上面一行命令行）

拷贝 id_rsa.pub 文件的内容，你可以用编辑器打开文件复制，也可以用 git 命令复制该文件的内容，如下：

mac端copy

```
pbcopy < ~/.ssh/github.pub
```



---

参考文档

https://blog.csdn.net/Felicity294250051/article/details/53606158

