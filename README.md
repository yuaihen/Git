## Git命令

### 设置用户名和邮箱地址

```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```


### Git初始化

```
git init
```

### Git添加文件

* 添加单个文件  

```
git add readme.txt
```

*  添加所有文件

```java
git add .
```

### Git查看状态

```
git status
```

### Git修改操作

* 查看版本历史记录

  ```
  git log --pretty=oneline
  ```

* 回退到指定版本

  ```
  git reset --hard commit_id  版本号没必要写全，前几位就可以了
  git reset --hard HEAD~1
  上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。
  ```

* 查看Git提交历史命令

  ```
  git reflog
  ```

* 比较文件区别

  ```
  git diff filename:比较工作区和暂存区
  
  git diff HEAD -- filename:比较工作区和版本库的最新版本
  ```

* 丢弃工作区的修改

  ```
  git checkout -- readme.txt
  回到最近一次git commit或git add时的状态
  从来没有被添加到版本库就被删除的文件，是无法恢复的！
  ```

* 丢弃暂存区的修改

  ```
  git reset HEAD readme.txt
  ```

  如果已经add到暂存区需要恢复，需要先执行reset再执行checkout

* 删除文件

  ```
  git rm test.txt
  ```

### GitHub远程仓库操作

* 创建SSH Key

  ```
   ssh-keygen -t rsa -C "youremail@example.com"
  ```

* 远程仓库与本地仓库关联

  ```
  git remote add origin git@github.com:用户名/仓库名.git
  添加第二个仓库也是这个命令
  ```

* 第一次推送

  ```
  git push -u origin master
  由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令
  ```

* 推送到远程仓库

  ```
  git push origin master
  origin仓库名 master分支名
  多个远程仓库需要不同的仓库名
  ```

* 删除已经添加的远程库：

  ```
  git remote rm origin(远程库名称)
  ```

### Git分支管理

* 创建并切换到新分支

  ```
  gut switch -c dev	//新版本命令
  
  git checkout -b dev	//旧的命令
  git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：
  $ git branch dev  	//创建分支
  $ git checkout dev  //切换分支 旧版本
  $ git switch dev	//切换分支 新版本
  Switched to branch 'dev'
  ```

* 合并分支

  ```
  git merge dev
  将dev分支内容合并到当前分支
  ```

* 合并分支禁用Fast forward 

  > 通常，合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。

  

  ```
  git merge --no-ff -m "merge with no-ff" dev
  因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去。
  ```

* 删除分支

  ```
  git branch -d dev
  
  如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除
  ```

* 查看分支合并情况

  ```
  git log --graph --pretty=oneline --abbrev-commit  //简略
  git log --graph	//详细信息
  ```

* 保存工作状态(未commit时切换分支需要保存工作状态)

  ```
  git stash  保存状态前需要先add追踪
  ```

* 恢复工作状态

  ```
  一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；
  另一种方式是用git stash pop，恢复的同时把stash内容也删了：
  ```

* 查看保存的工作状态

  ```
  git stash list
  多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash
  
  git stash apply stash@{0}
  ```

* 复制一个特定的提交到当前分支

  ```
  git cherry-pick 4c805e2 
  ```

* 查看远程库信息

  ```
  git remote 
  git remote -v  查看详细信息
  ```

* 本地推送分支要远程库

  ```
  使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交
  ```

* 本地创建远程分支对应的分支

  ```
  git checkout -b branch-name origin/branch-name
  git switch -c branch-name origin/branch-name
  本地和远程分支的名称最好一致
  ```

* 建立本地分支和远程分支的关联

  ```
  git branch --set-upstream branch-name origin/branch-name
  ```

* 从远程抓取分支

  ```
  git pull 
  ```

* 删除远程仓库分支

  ```
  git push origin --delete dev
  ```

* #### Rebase

  ```
  Git rebase   在push之前操作 
  把分叉的提交历史“整理”成一条直线，看上去更直观。缺点是本地的分叉提交已经被修改过了。
  ```


### 标签管理

* 创建Tag

  ```
  git tag <name> 默认标签是打在最新提交的commit上的
  
  git tag v0.9 f52c633     //对指定的commit id打tag
  
  git tag -a v0.1 -m "version 0.1 released" 1094adb	//创建带有说明的标签
  ```

* 查看标签

  ```
  git tag
  
  git show <tagname>		//查看标签信息
  ```

* 删除标签

  ```
  git tag -d v0.1
  ```

* 推送标签到远程库

  ```
  git push origin <tagname>		//推送某个标签
  git push origin --tags			//推送全部标签
  ```

* 删除远程库标签

  ```
  git tag -d v0.9		//先删除本地标签
  git push origin :refs/tags/v0.9	//删除远程标签
  ```

### 添加忽略文件

* 强制添加已经被忽略的文件 `-f`

  ```
  git add -f App.class
  ```

* 检查忽略文件规则    `git check-ignore`

  ```
  $ git check-ignore -v App.class
  .gitignore:3:*.class	App.class
  ```

  `.gitignore`文件本身要放到版本库里，并且可以对`.gitignore`做版本管理

### 配置别名

* ```
  我们只需要敲一行命令，告诉Git，以后st就表示status：
  $ git config --global alias.st status
  ```

  每个仓库的Git配置文件都放在`.git/config`文件中：

  ```
  $ cat .git/config 
  ```

  而当前用户的Git配置文件放在用户主目录下的一个隐藏文件`.gitconfig`中：

  ```
  $ cat .gitconfig
  ```

  

