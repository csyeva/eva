# git入门使用二
>* Git服务器与远程操作

# Git服务器与远程操作
>* git remote add
>* git clone
>* git push
>* git remote –v
>* ssh 生成公钥私钥文件
>* git fetch
>* git merge
>* git pull


什么要用到Git服务器
>* Git版本数据也是保存在自己的电脑上，这其实非常不安全，因为你可能会感染电脑病毒，会错误删除文件，危害到了git版本数据。
>* 所以我们在本机上保存版本数据，最好的备份方式就是使用git服务器。
>* 使用git服务器不仅保证数据的安全性，还能够多人共享，多人协同开发项目。

## 协议
1. SSH 协议 1
```shell
ssh://[user@]example.com:[:port]/path/to/repo.git/

```
>* 可以在URL中设置用户名和端口。默认端口为22

2. SSH协议 2
```shell
[user@]example.com:[:port]/path/to/repo.git/

```
>* SCP格式表示法，更简洁。但是非默认端口需要通过其他方式（如主机别名方式）

3. GIT协议
```shell
git://example.com[:port]/path/to/repo.git/

```

4. HTTP[S]协议
```shell
http[s]://example.com:[port]/path/to/repo.git

```
>* 兼有智能协议和哑协议


> 还支持其他协议如FTP,RSYNC(这两种属于哑协议)，SSH和GIT协议属于智能协议。两者的区别，我们明白一点就是哑协议：传输速度非常慢，传输进度不可见，不知道什么时候数据传输完成。而智能协议，传输速度快，可以看到传输进度。

## Git服务器与远程操作

>* bitbucket.org
>* github.com

演示bitbucket.org的操作 首先要生成公钥和私钥
```shell
1.用SSH生成公钥和私钥
ssh-keygen -t rsa -C “你配置的电子邮件”
ssh-keygen -t rsa -C “tangseng2013git@163.com”
2.把生成的公钥文件用记事本之类的文本编辑软件打开，复制到网站相应的key中

```

```shell
测试SSH公钥是否成功
ssh git@git服务器地址

```

演示bitbucket.org的操作
>* 1.在bitbucket.org上创建一个新项目仓库，克隆这个项目，在本地添加源代码之后，推送上去。
>* 2.在刚才bitbucket.org项目上，演示2个人参与的情况。学习git fetch,git merge,git pull命令。




