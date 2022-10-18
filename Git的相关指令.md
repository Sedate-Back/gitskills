# Git的相关指令

## 一. 下载与安装

1. https://git-scm.com/downloads进入后选择版本，下载安装

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
4. 然后使用这个命令把内容推送给github的库`git push -u origin master/main`
   1. 这里需要注意，github上面创建的文件夹名称，后面统一用main
5. 以后更新就用`git push origin master`
6. 如果仓库写错了，可以用`git remote rm <name>`，使用前可以先看远程库的信息
   1. `git remote -v`

## 七. 分支管理

1. git里有分支的概念，一般情况下我们初始化之后的Git路径是只有master/main，然后修改文档或者数据后，会由HEAD（原始版本）指向master/main，提交之后就会将HEAD的内容提交给master/main；
2. 假设我们创建新的分支，例如dev时，Git此时的HEAD就会指向这个分支dev，dev和master/main一起指向管理版本。此时由于我们在控制的是HEAD链路，所以更新、提交和上传等操作，都是围绕dev这条路线进行的
3. 假设我们想要将分支上的dev的内容合并到初始的master/main上，我们也可以通过代码合并，合并之后可以删除dev分支，毕竟合并后的内容是一致的
4. 相关代码

```python
git checkout -b dev # 创建分支，切换到分支

git switch -c dev # 创建分支，切换到分支
git switch master/main # 切换已有的分支

git branch # 查看当前分支

# 提交
git add readme.txt
git commit -m "branch.txt" 

# 切换回master/main
git checkout master/main

# 合并与删除 这种合并方式是快速合并和删除，该分支的记录也会被删除的一干二净
git merge dev
git branch -d dev

# 再次查看就只有master/main
git branch
```

5. 当我们在分支上修改文档后，又在master/main中修改了不一样的版本，并且两份不同的文件已经提交了，在合并的时候Git会让我们解决内容不一样的冲突之后才能合并，一般情况下就需要去文档里修改，修完提交，才能合并`merge`和删除`branch -d`，当然，分支的结构也会改变

6. 不进行快删除的方式合并和删除分支，需要`--no-ff`，这种方式会合并并创建一个新的commit，在历史上留下痕迹

   1. ```python
      git switch -c dev
      git add readme.txt
      git commit -m "add merge"
      git switch master/main
      git merge --no-ff -m "merge with no-ff" dev
      ```

7. 当我们临时收到要改bug的时候，我们还有文件存在分支dev上，这时候就需要暂时保存dev分支上的内容并把暂存区清空，再创建bug分支，来做临时处理

   1. ```python
      git stash # 储藏内容
      git checkout master/main
      
      git checkout -b issue-101
      git add readme.txt
      git commit -m "fix bug 101"
      
      git switch master
      git merge --no-ff -m "merge bug fix 101" issue-101
      
      git switch dev
      
      # 释放内容
      git stash list # 查看存储的内容
      # 1.先应用后删除
      git stash apply
      git stash drop
      
      # 2.一步到位
      git stash pop
      
      ```

   2. 但是我们修改了bug之后，需要将这个bug同步给我们的dev分支，就需要将提交给master/main的内容，轻提交给dev分支

      1. ```python
         git branch
         git cherry-pick (commit id)
         ```

8. 开发功能节点附加到版本上的时候，可以新建一个分支作为版本分支，然后将这个版本分支和dev分支进行合并，但是将版本内容上传和提交到git之后，要与dev合并，如果不想合并想要删除，怎么办？

   1. ```python
      git switch -c feature-vulcan
      
      git add xxx.txt
      git commit -m "add xxx file"
      
      git switch dev
      
      git branch -D feature-vulan # 强制删除feature-vulan分支
      ```

## 八. 多人协作

1. 流程：

   1. 首先，可以试图用`git push origin <branch-name>`推送自己的修改；
   2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
   3. 如果合并有冲突，则解决冲突，并在本地提交；
   4. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！
   5. 如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。

   这就是多人协作的工作模式，一旦熟悉了，就非常简单。

   

