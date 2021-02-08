### 本地项目关联到远端git仓库

1. 输入“git remote add origin https://github.com/你的用户名/Test.git”（git remote add origin 你自己的https地址），连接你的guthub仓库。
2. 输入“git push -u origin master”，上传项目到Github。这里会要求输入Github的账号密码，按要求输入就可以。

### 忽略本地修改，强制拉取远程到本地

```bash
git fetch --all
git reset --hard origin/dev
git pull
```



git pull --rebase origin master

