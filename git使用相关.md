更换电脑，git重新认证：
1.配置用户名+邮箱：
git config --global user.name [username]
git config --global user.email [email]

2.重置用户名+密码
git config --system --unset credential.helper

3.存储用户名+密码
git config --global credential.helper store
git pull
然后就会要求输入用户名(邮箱)+密码，git就会保存了




```
配置远程仓库 
git remote add origin http://xxx.git
clone 远程仓库指定分支
git clone -b 1.1.1_xxx  http://xxx.git
查看全部分支
git branch -a

切换分支
git checkout 分支名

开始推送
git push <远程主机名> <本地分支名>:<远程分支名>
git push origin master:master

删除远程分支
git push origin --delete dev

删除本地分支
git branch -d dev



git checkout -b 1.1.1_lwh remotes/origin/1.1.1_lwh



1、如果远程新建了一个分支，本地没有该分支。
可以利用 git checkout --track origin/branch_name ，这时本地会新建一个分支名叫 branch_name ，会自动跟踪远程的同名分支 branch_name。
git checkout --track origin/branch_name
git checkout --track remotes/origin/1.1.1_lwh01


2.如果本地新建了一个分支 branch_name，但是在远程没有。
这时候 push 和 pull 指令就无法确定该跟踪谁，一般来说我们都会使其跟踪远程同名分支，所以可以利用 git push --set-upstream origin branch_name ，这样就可以自动在远程创建一个 branch_name 分支，然后本地分支会 track 该分支。后面再对该分支使用 push 和 pull 就自动同步。
git push --set-upstream origin branch_name
```

# git合并时冲突

```visual basic
<<<<<<< HEAD
new new new new code(本地代码)
=======
old old old code（拉下来的代码）
>>>>>>> xxxxxxxxxxxxxxxxxxxxxxx


分析：head 到 =======里面的lalala是自己的commit的内容
```

