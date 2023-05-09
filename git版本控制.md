# git的版本控制

## 创建版本库

1. 初始化版本库

   ```
   git init
   ```

2. 添加文件到git管理工具中

   ```
   git readme.txt  # 添加readme.txt文件到git管理中
   
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
   ```

4. 版本回退

   ```
   git reset --hard HEAD^  # 一个^表示回退一个版本 ^^表示回退两个版本 以此类推
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

