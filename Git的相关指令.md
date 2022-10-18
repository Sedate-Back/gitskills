# Git的相关指令

## 一. 下载与安装

## 二. Git初始化

1. `git config --global user.name "SedateAiot"`
2. `git config --global user.email "sedate_0521@qq.com"`
3. `cd E:\Luoy_project_git\ ` 进入要用Git管理的文件夹中
4. `mkdir aiot`  新建文件夹
5. `cd aiot`  进入
6. `git init` 初始化git

## 三. 安全认证与上传

1. `git config --global --add safe.directory "*"`
2. `git add xxxx.txt`
3. `git commit -m "add readme.txt"`
4. 这样就提交成功了！

## 四. 编辑文件后查看是否上传、查看修改内容、再次上传等；

1. 可以查看aiot的状态 `git status`
2. 可以查看刚刚传进去的 xxxx.txt 文件修改在哪个地方`git diff xxxx.txt`
3. 在本地修改完xxxx.txt文件之后，如果需要上传给git，需要重复执行5.2和5.3的代码

## 五. 也可将文件退回到某个版本

1. `git log --pretty=oneline` 日志
2. `git reset --hard HEAD^` 退回上一个版本
3. `cat readme.txt` 获取当前readme文档的内容
4. 假设回退之后，不想要回退了，可以找到之前的commit id，`git reset --hard (commit id)` 就可以退回之前版本
   1. 处理查看历史记录找到这个commit id
   2. 还可以查看历史命令 `git reflog`
5. 例如我们在Git文件夹下改了文件，想要将文件退回到原始版本，可以用`git checkout -- readme.txt`复原文件
6. 如果添加到了暂存区，但是还没有commit，可以用`git reset HEAD readme.txt`  删除暂存区里的内容
7. 我们把新文件传给git之后，后续用filter把文件删除后，需要`git rm test.txt` & `git commit -m 'xxxx'`来把工作区的文件彻底删除；
8. 误删了需要的文件，但是文件已经commit给git的话，就可以`git checkout -- test.txt` 还原

## 六. 远程仓库

1. 关联我自己的github库，需要在账号页面添加本地的SSHkey值给SSH页面
2. 之后再用bash输入`git remote add origin git@SedateAiot/aiot.git`
3. 生成的origin是远程库
4. 然后使用这个命令把内容推送给github的库`git push -u origin master`
5. 以后更新就用`git push origin master`
6. 如果仓库写错了，可以用`git remote re <name>`，使用前可以先看远程库的信息
   1. `git remote -v`

