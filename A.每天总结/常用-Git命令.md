## github常用命令

```shell
git init;
git status;
git add .;
git commit -m "一些说明";
git remote add origin github地址;
git push -u origin master;
git checkout -b  分支名字
git clone github仓库地址
git clone -b 分支名字 github仓库地址
git log
git reset --hard commit的id
```

联合命令

```shell
cd 目标目录; git add .;git commit -m "提交所有修改过的";git push -u origin master;
```

拉取远程分支覆盖本地分支

```shell
git fetch --all; git reset --hard origin/master;
```

