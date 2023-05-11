# git的版本控制

## 创建版本库

1. 初始化版本库

   ```
   git init
   ```

2. 添加文件到git管理工具中

   ```
   git add readme.txt  # 添加readme.txt文件到git管理中
   
   git add .  # 将当前所有的文本都提交到git管理中 
   不建议全部提交git管理
   ```

3. 提交到仓库

   ```
   git commit -m "wrote a readme file"
   # -m后面是写对本次提交的说明  可以不写 但是最好写、
   ```

   



## git操作

1. 查看文件状态

   ```
   git status
   ```

2. 查看文件的不同

   ```
   git diff
   ```

3. 查看历史提交记录

   ```
   git log
   
   # 若输出的信息太多，可以使用
   git log --pretty=oneline
   
   # 若记录太多可以使用
   git log -i  # i是你想看见的最近几条记录 如-2就是看两条
   
   # 以图的形式查看
   git log --graph
   ```

4. 版本回退

   ```
   git reset --hard HEAD^  # 一个^表示回退一个版本 ^^表示回退两个版本 以此类推
   
   # 也可以直接使用commit id来回退
   ```

5. 版本找回

   ```
   # 当版本回退后，使用git log 会发现原来的版本不见了
   # 若当前窗口未关闭,找到之前git log 前面的版本号，一般是乱码，输入前面几个字母即可 git会自动查找
   
   git reset --hard aa20f  # 例如这样
   
   ```

6. reflog命令

   ```
   # 当关闭了窗口，仍想找回版本， 则可以使用git reflog命令查看 过去版本变化的命令
   
   git reflog
   ```

7. 文件还没git add时，想要恢复成上一次commit的样子

   ```
   # git checkout -- readme.txt  # 就用restore吧  统一一点
   
   # 就用这个  这个功能包括文件的删除
   git restore readme.txt
   ```

8. 当文件git add了之后，将其恢复成没有git add的状态

   ```
   git restore --staged readme.txt
   
   # 这个功能包括文件的删除
   # 若文件git add了之后，想要删除，方法就是先从git add 拿出来 在将没git add的文件进行恢复
   ```

   

9. 

## 远程仓库

参考链接:https://www.liaoxuefeng.com/wiki/896043488029600/896954117292416

由于本地仓库和远程仓库传输通过SSH协议传输，因此需要创建SSH Key：

1. 打开终端

   ```
   ssh-keygen -t rsa -C "youremail@example.com"
   ```

   

2. 一路回车或者为空

3. 登录github 点击账户设置

4. 找到SSH Key并点击添加，其中title可以随便写

5. 在用户主目录里面(隐藏文件)中找到.ssh目录，将id_rsa.pub文件中的内容复制并粘贴到key文本库内

6. 点击添加即可



### 添加远程仓库

1. 在github上创建一个仓库

2. 使用命令关联

   ```
   git remote add origin https://github.com/ciaoyizhen/learn_git.git  # 其中learn_git是项目名
   ```

3. 推送文件

   ```
   git push -u origin main  # -u是在第一次提交时使用，目的是进行绑定 之后已经绑定了则不需要使用-u命令
   ```



建议使用github自带的命令

![image-20230509203502835](./git%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6.assets/image-20230509203502835.png)

>非常需要注意的是，github已经从2021年8月13日开始，不再使用push密码的方式了

使用个人token教程链接:https://www.cnblogs.com/chenyablog/p/15397548.html

这个token在一个机器上 输入过一次即可被记住。



### 删除远程仓库

```
# 查看远程库信息
git remote -v

# 根据其结果中的名字进行删除
git remove rm origin

# 解除关联，并非真的删除
```



### 从远程仓库克隆

```
使用命令  git clone 地址
```





## 分支管理

### 创建和合并分支

```
# 创建分支
git branch dev  # dev表示分支名

# 查看分支
git branch

# 切换分支
git switch dev  # dev 表示分支名

# 合并分支
git merge dev  # dev 表示分支名

# 删除分支
git branch -d dev # dev为分支名  若东西没有合并就删除 可能会报错，这时可以使用-D来操作
```



### 解决冲突

当两个分支内的代码不是简单的增加或删除，而是都有部分的修改时，在合并分支时，会报错。

这时，直接打开文件，会将不同的地方，git都会指出来，然后进行手动修改。

修改完成后 进行 add和commit的操作即可。

最后删除分支即可。



### git stash功能

若新增了一个分支，并往里添加了一个新文件，切回主分支，新文件也会存在(即使使用了add也是一样，只有commit了才会不在别的分支看到)。

但这时想切换成别的分支，且需要该分支原始的样子，即没有新文件。

这时需要使用(需要先将文件进行add，因为只有add了，git才对其进行管理)

```
git stash  # 该功能是将文件的修改隐藏，恢复成commit的样子

git stash pop  # 恢复隐藏 并删除这个隐藏

# 当有多个隐藏时
git stash list  # 查看隐藏

# index是指 git stash list中的结果
git stash apply stash@{index}  # 恢复隐藏，但不删除隐藏
git stash drop stash@{index}  # 删除隐藏

```



### 多人开发

当一个人提交修改代码到仓库中，你也修改了代码，这时，想要提交代码是会出现问题的。

此时，操作如下:

1. 进行关联操作

   ```
   git branch --set-upstream-to=origin/仓库分支名 你的分支
   # 这个是将远程仓库的分支和你的分支关联起来
   # 例如 git branch --set-upstream-to=origin/main dev 是将仓库的主分支和你的dev分支关联起来
   ```

   

2. 将前一个人修改的代码拉取下来

   ```
   git pull  # 要在你的关联的分支下
   ```

   

3. 可能会出现的情况(没出现则直接跳到6)

   ```
   hint: You have divergent branches and need to specify how to reconcile them.
   hint: You can do so by running one of the following commands sometime before
   hint: your next pull:
   hint:
   hint:   git config pull.rebase false  # merge
   hint:   git config pull.rebase true   # rebase
   hint:   git config pull.ff only       # fast-forward only
   hint:
   hint: You can replace "git config" with "git config --global" to set a default
   hint: preference for all repositories. You can also pass --rebase, --no-rebase,
   hint: or --ff-only on the command line to override the configured default per
   hint: invocation.
   fatal: Need to specify how to reconcile divergent branches.
   ```

   翻译如下：

   

4. 进行对应的操作

   ```
   # 例如
   git config pull.rebase false
   ```

   

5. 在进行拉去

   ```
   git pull
   ```

   

6. 进行修改(例如修改两个冲突的地方)

7. 提交

   

### 变基操作

如何将多个commit结果合并成一个

```
git rebase -i HEAD~number  # number为你要合并的数量，并且是以HEAD开始

之后会进入vi编辑

将第一个pick保留 其余的改为s  # 具体的可以看下面的提示命令
按esc键 :wq保存退出
然后进入一个编辑提交信息的vi编辑
然后将原先的提交信息删除，写上这一次的
按esc ：wq保存退出即可
```









## 标签管理


标签管理就是对某次提交打个醒目的记号，如v1.0这样的标签。

### 创建标签

```
git tag v1.0  # 默认是打在当前上

# 打在过去的commit上
git tag 0.9  commit_id # commit_id表示你要打标签的commit

# 查看标签列表
git tag

# 查看标签详细信息
git show v1.0

# 附注标签
git tag -a v0.1 -m "说明文档"  # 同样是标记在当前的提交上，可以使用commit_id

```



### 操作标签

```
git tag -d v1.0  # 删除标签

git push origin v1.0  # 将标签推送到远程

git push origin -d tag v1.0  # 将远程的标签删除
```



## github操作

fork命令

当一个项目你想进行添砖加瓦时，你需要先将其fork进自己的仓库，然后git clone 进行修改提交，然后对作者发起pull request即可。



gitee和github是一样的，其区别在于国内网络访问gitee稳定，就不说了。



## 自定义git

### 忽略特殊文件

在git工作区的根目录下创建一个特殊的 .gitignore文件，将要忽略的文件放入，git就会忽略这些文件。

哪些文件适合忽略？

1. 操作系统产生的文件
2. 编译产生的中间文件，日志等
3. 敏感信息





>廖雪峰的git学习网站还有一点点的细枝末节，个人觉得不重要，就不做了，需要则自行百度，上述的命令已经非常全面了。

