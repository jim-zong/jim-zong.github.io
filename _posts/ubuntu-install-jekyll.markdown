ubuntu 16.04安装jekyll 构建个人博客
***

### Content

* 前言
    - Jekyll是什么 ?
* 安装
    - rvm
    - 使用rvm 安装ruby
    - 使用gem安装jekyll
* 使用
    - jekyll快速上手, 基本用法
  
* 总结
    - 实现把你的个人博客推送github.io 上
* 其他
    - 问题 下载太慢(网络故障)，gem换源

* 参考文献
    - [ubuntu 16.04安装jekyll自动化部署个人网站](https://xjtushilei.github.io/2017/05/19/jekyllinstall/#/repo)
    - [Ubuntu 16.04 安装使用 Jekyll](http://blog.topspeedsnail.com/archives/7583#/repo)


# 前言

### Jekyll是什么？

```
Jekyll是一个使用ruby编写的简单的静态网站生成器，它会根据网页源码、Markdown生成静态文件，不需要数据库支持。它提供了模板、插件等功能，所以实际上可以用来编写整个网站。它是Github Page官方指定的工具，非常适合搭建简单的博客。因此我们首先需要搭建一个ruby环境。
```


###系统环境


> ubuntu@VM-155-46-ubuntu:~$ cat /etc/issue
Ubuntu 16.04.1 LTS \n \l



### 新建用户

`ubuntu@VM-155-46-ubuntu:~$ sudo adduser deploy
Adding user `deploy' ...
Adding new group `deploy' (1002) ...
Adding new user `deploy' (1002) with group `deploy' ...
Creating home directory `/home/deploy' ...
Copying files from `/etc/skel' ...
Enter new UNIX password: 
Retype new UNIX password: 
passwd: password updated successfully
Changing the user information for deploy
Enter the new value, or press ENTER for the default
	Full Name []: 
	Room Number []: 
	Work Phone []: 
	Home Phone []: 
	Other []: 
Is the information correct? [Y/n] Y`



