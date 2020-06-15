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
```

