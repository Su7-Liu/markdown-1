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
git remote add origin http://gitlab.sto.cn/devops/idc_cmdb.git
git clone -b 1.1.1_lwh01  http://gitlab.sto.cn/devops/idc_cmdb.git
git branch -a
```