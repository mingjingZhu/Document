#### 本地项目关联到远端git仓库

1. 输入“git remote add origin https://github.com/你的用户名/Test.git”（git remote add origin 你自己的https地址），连接你的guthub仓库。
2. 输入“git push -u origin master”，上传项目到Github。这里会要求输入Github的账号密码，按要求输入就可以。

#### 忽略本地修改，强制拉取远程到本地

```bash
git fetch --all
git reset --hard origin/dev  (dev: 分支名)
git pull
```

#### git pull 采用rebase not merge

git pull --rebase origin master

#### git commit –amend

> 既可以对上次提交的内容进行修改，也可以修改提交说明。
>
> git commit --amend 相当于上次提交错误的信息被覆盖了，gitk图形化界面上看不到上次提交的信息，git log上也看不到之前的信息。

1. 修改提交内容：git commit –amend –no-edit
2. 修改提交说明：git commit --amend   --->  输入i，修改commit说明  /  git commit -a --amend -m "my message here"



