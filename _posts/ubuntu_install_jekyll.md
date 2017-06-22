ubuntu 16.04安装jekyll 构建个人博客
===========

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

ubuntu@VM-155-46-ubuntu:~$ cat /etc/issue
Ubuntu 16.04.1 LTS \n \l



### 新建用户

```
ubuntu@VM-155-46-ubuntu:~$ sudo adduser deploy
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
Is the information correct? [Y/n] Y
```



### 授权sudo用户

```
ubuntu@VM-155-46-ubuntu:~$ sudo usermod -a -G sudo deploy
```



### 切换到deploy 新用户下


	ubuntu@VM-155-46-ubuntu:~$ su - deploy 
	Password: 
	To run a command as administrator (user "root"), use "sudo <command>".
	See "man sudo_root" for details.
	deploy@VM-155-46-ubuntu:~$ exit
	logout
	ubuntu@VM-155-46-ubuntu:~$ sudo su - deploy 
	To run a command as administrator (user "root"), use "sudo <command>".
	See "man sudo_root" for details.



### 安装rvm


	deploy@VM-155-46-ubuntu:~$ curl -L get.rvm.io | bash -s stable 
	  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
	                                 Dload  Upload   Total   Spent    Left  Speed
	100   194  100   194    0     0     96      0  0:00:02  0:00:02 --:--:--    96
	100 24063  100 24063    0     0   7990      0  0:00:03  0:00:03 --:--:--  166k
	Downloading https://github.com/rvm/rvm/archive/1.29.1.tar.gz
	Downloading https://github.com/rvm/rvm/releases/download/1.29.1/1.29.1.tar.gz.asc
	Found PGP signature at: 'https://github.com/rvm/rvm/releases/download/1.29.1/1.29.1.tar.gz.asc',
	but no GPG software exists to validate it, skipping.
	
	Installing RVM to /home/deploy/.rvm/
	    Adding rvm PATH line to /home/deploy/.profile /home/deploy/.mkshrc /home/deploy/.bashrc /home/deploy/.zshrc.
	    Adding rvm loading line to /home/deploy/.profile /home/deploy/.bash_profile /home/deploy/.zlogin.
	Installation of RVM in /home/deploy/.rvm/ is almost complete:
	
	  * To start using RVM you need to run `source /home/deploy/.rvm/scripts/rvm`
	    in all your open shell windows, in rare cases you need to reopen all shell windows.
	
	# deploy,
	#
	#   Thank you for using RVM!
	#   We sincerely hope that RVM helps to make your life easier and more enjoyable!!!
	#
	# ~Wayne, Michal & team.
	
	In case of problems: https://rvm.io/help and https://twitter.com/rvm_io
	deploy@VM-155-46-ubuntu:~$ echo $?
	0


添加系统环境变量


	deploy@VM-155-46-ubuntu:~$ sudo vim /etc/profile
	[sudo] password for deploy: 
	deploy@VM-155-46-ubuntu:~$ ll /home/deploy/.rvm/scripts/rvm
	-rwxrwxr-x 1 deploy deploy 7663 Jun 22 14:15 /home/deploy/.rvm/scripts/rvm*
	deploy@VM-155-46-ubuntu:~$ sudo vim /etc/profile
	deploy@VM-155-46-ubuntu:~$ ll /home/deploy/.rvm/bin
	total 36
	drwxrwxr-x  2 deploy deploy 4096 Jun 22 14:15 ./
	drwxrwxr-x 25 deploy deploy 4096 Jun 22 14:15 ../
	lrwxrwxrwx  1 deploy deploy    9 Jun 22 14:15 ruby-rvm-env -> rvm-shell*
	-rwxrwxr-x  1 deploy deploy 1534 Jun 22 14:15 rvm*
	-rwxrwxr-x  1 deploy deploy 1799 Jun 22 14:15 rvm-auto-ruby*
	-rwxrwxr-x  1 deploy deploy 2059 Jun 22 14:15 rvm-exec*
	-rwxrwxr-x  1 deploy deploy 3983 Jun 22 14:15 rvm-prompt*
	lrwxrwxrwx  1 deploy deploy   13 Jun 22 14:15 rvm-shebang-ruby -> rvm-auto-ruby*
	-rwxrwxr-x  1 deploy deploy 2224 Jun 22 14:15 rvm-shell*
	-rwxrwxr-x  1 deploy deploy  648 Jun 22 14:15 rvm-smile*
	-rwxrwxr-x  1 deploy deploy 2618 Jun 22 14:15 rvmsudo*
	deploy@VM-155-46-ubuntu:~$ source /etc/profile

```
deploy@VM-155-46-ubuntu:~$ source /home/deploy/.rvm/scripts/rvm
```


查看能安装的ruby版本

	deploy@VM-155-46-ubuntu:~$ rvm list known
	# MRI Rubies
	[ruby-]1.8.6[-p420]
	[ruby-]1.8.7[-head] # security released on head
	[ruby-]1.9.1[-p431]
	[ruby-]1.9.2[-p330]
	[ruby-]1.9.3[-p551]
	[ruby-]2.0.0[-p648]
	[ruby-]2.1[.10]
	[ruby-]2.2[.6]
	[ruby-]2.3[.3]
	[ruby-]2.4[.0]
	ruby-head
	
	# for forks use: rvm install ruby-head-<name> --url https://github.com/github/ruby.git --branch 2.2
	
	# JRuby
	jruby-1.6[.8]
	jruby-1.7[.26]
	jruby[-9.1.7.0]
	jruby-head





查看系统当前所用的ruby版本

```
deploy@VM-155-46-ubuntu:~$ ruby -v
ruby 2.3.1p112 (2016-04-26) [x86_64-linux-gnu]
```


安装ruby2.4.0版本


	deploy@VM-155-46-ubuntu:~$ rvm install ruby-2.4.0
	Searching for binary rubies, this might take some time.
	Found remote file https://rvm_io.global.ssl.fastly.net/binaries/ubuntu/16.04/x86_64/ruby-2.4.0.tar.bz2
	Checking requirements for ubuntu.
	Installing requirements for ubuntu.
	Updating system....
	Installing required packages: libyaml-dev, sqlite3, autoconf, libgdbm-dev, libncurses5-dev, automake, libtool, bison, pkg-config, libffi-dev............
	Requirements installation successful.
	ruby-2.4.0 - #configure
	ruby-2.4.0 - #download
	  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
	                                 Dload  Upload   Total   Spent    Left  Speed
	100 16.4M  100 16.4M    0     0  19935      0  0:14:24  0:14:24 --:--:-- 36743
	ruby-2.4.0 - #validate archive
	ruby-2.4.0 - #extract
	ruby-2.4.0 - #validate binary
	ruby-2.4.0 - #setup
	ruby-2.4.0 - #gemset created /home/deploy/.rvm/gems/ruby-2.4.0@global
	ruby-2.4.0 - #importing gemset /home/deploy/.rvm/gemsets/global.gems..............................
	ruby-2.4.0 - #generating global wrappers........
	ruby-2.4.0 - #gemset created /home/deploy/.rvm/gems/ruby-2.4.0
	ruby-2.4.0 - #importing gemsetfile /home/deploy/.rvm/gemsets/default.gems evaluated to empty gem list
	ruby-2.4.0 - #generating default wrappers........
	deploy@VM-155-46-ubuntu:~$ echo $?
	0












	deploy@VM-155-46-ubuntu:~$ rvm gemlist
	Unrecognized command line argument: 'gemlist' ( see: 'rvm usage' )
	deploy@VM-155-46-ubuntu:~$ rvm list
	
	rvm rubies
	
	   ruby-2.4.0 [ x86_64 ]
	
	# Default ruby not set. Try 'rvm alias create default <ruby>'.
	
	# => - current
	# =* - current && default
	#  * - default








把ruby2.4.0设置成当前默认版本

```
deploy@VM-155-46-ubuntu:~$ rvm use ruby-2.4.0 --default 
Using /home/deploy/.rvm/gems/ruby-2.4.0
deploy@VM-155-46-ubuntu:~$ ruby -v
ruby 2.4.0p0 (2016-12-24 revision 57164) [x86_64-linux]
```



### gem换源

```
deploy@VM-155-46-ubuntu:~$ gem sources --remove https://rubygems.org/
https://rubygems.org/ removed from sources
deploy@VM-155-46-ubuntu:~$ gem sources --add https://gems.ruby-china.org/
https://gems.ruby-china.org/ added to sources
```




### 安装 nodejs


	deploy@VM-155-46-ubuntu:~$ curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
	[sudo] password for deploy: 
	
	## Installing the NodeSource Node.js v6.x repo...
	
	
	## Populating apt-get cache...
	
	+ apt-get update
	Hit:1 http://mirrors.tencentyun.com/ubuntu xenial InRelease
	Hit:2 http://mirrors.tencentyun.com/ubuntu xenial-security InRelease
	Hit:3 http://mirrors.tencentyun.com/ubuntu xenial-updates InRelease
	Reading package lists... Done
	
	## Confirming "xenial" is supported...
	
	+ curl -sLf -o /dev/null 'https://deb.nodesource.com/node_6.x/dists/xenial/Release'
	
	## Adding the NodeSource signing key to your keyring...
	
	+ curl -s https://deb.nodesource.com/gpgkey/nodesource.gpg.key | apt-key add -
	OK
	
	## Creating apt sources list file for the NodeSource Node.js v6.x repo...
	
	+ echo 'deb https://deb.nodesource.com/node_6.x xenial main' > /etc/apt/sources.list.d/nodesource.list
	+ echo 'deb-src https://deb.nodesource.com/node_6.x xenial main' >> /etc/apt/sources.list.d/nodesource.list
	
	## Running `apt-get update` for you...
	
	+ apt-get update
	Hit:1 http://mirrors.tencentyun.com/ubuntu xenial InRelease
	Hit:2 http://mirrors.tencentyun.com/ubuntu xenial-security InRelease
	Hit:3 http://mirrors.tencentyun.com/ubuntu xenial-updates InRelease
	Get:4 https://deb.nodesource.com/node_6.x xenial InRelease [4634 B]
	Get:5 https://deb.nodesource.com/node_6.x xenial/main Sources [764 B]
	Get:6 https://deb.nodesource.com/node_6.x xenial/main amd64 Packages [962 B]
	Get:7 https://deb.nodesource.com/node_6.x xenial/main i386 Packages [961 B]
	Fetched 7321 B in 3s (2348 B/s)
	Reading package lists... Done
	
	## Run `apt-get install nodejs` (as root) to install Node.js v6.x and npm
	
	deploy@VM-155-46-ubuntu:~$ echo $?
	0
	deploy@VM-155-46-ubuntu:~$ sudo apt-get install -y nodejs
	Reading package lists... Done
	Building dependency tree       
	Reading state information... Done
	The following NEW packages will be installed:
	  nodejs
	0 upgraded, 1 newly installed, 0 to remove and 167 not upgraded.
	Need to get 10.1 MB of archives.
	After this operation, 50.8 MB of additional disk space will be used.
	Get:1 https://deb.nodesource.com/node_6.x xenial/main amd64 nodejs amd64 6.11.0-1nodesource1~xenial1 [10.1 MB]
	18% [1 nodejs 2293 kB/10.1 MB 23%]                                                                                                                                                                              43.6 kB/s 2min 58s19% [1 nodejs 2326 kB/10.1 MB 23%]                                                                                                                                                                              43.6 kBFetched 10.1 MB in 3min 35s (46.7 kB/s)                                                                                                                                                                                         
	Selecting previously unselected package nodejs.
	(Reading database ... 84943 files and directories currently installed.)
	Preparing to unpack .../nodejs_6.11.0-1nodesource1~xenial1_amd64.deb ...
	Unpacking nodejs (6.11.0-1nodesource1~xenial1) ...
	Processing triggers for man-db (2.7.5-1) ...
	Setting up nodejs (6.11.0-1nodesource1~xenial1) ...
	deploy@VM-155-46-ubuntu:~$ echo $?
	0
	deploy@VM-155-46-ubuntu:~$ jekyll -v
	jekyll 3.5.0


### 创建网站目录


	deploy@VM-155-46-ubuntu:~$ mdkir site
	No command 'mdkir' found, did you mean:
	 Command 'mdir' from package 'mtools' (main)
	 Command 'mkdir' from package 'coreutils' (main)
	mdkir: command not found
	deploy@VM-155-46-ubuntu:~$ mkdir site
	deploy@VM-155-46-ubuntu:~$ cd site/


### 使用jekyll创建一个静态博客
	deploy@VM-155-46-ubuntu:~/site$ jekyll new mySite
	Running bundle install in /home/deploy/site/mySite... 
            ...部分省略...
	  Bundler: Installing jekyll-feed 0.9.2
	  Bundler: Bundle complete! 4 Gemfile dependencies, 21 gems now installed.
	  Bundler: Use `bundle info [gemname]` to see where a bundled gem is installed.
	New jekyll site installed in /home/deploy/site/mySite. 
	deploy@VM-155-46-ubuntu:~/site$ echo $?
	0


### 安装依赖包

```
bundle install
```


### 启动jekyll

```
bundle exec jekyll serve 
```










###生成deploy的密钥



	deploy@VM-155-46-ubuntu:~/site/mySite$ git config --global user.email "jim-zong@17llife.com"
	deploy@VM-155-46-ubuntu:~/site/mySite$ git config --global user.name "jim-zong"
	deploy@VM-155-46-ubuntu:~/site/mySite$ ssh-keygen -t rsa -C "jim-zong@17llife.com"
	Generating public/private rsa key pair.
	Enter file in which to save the key (/home/deploy/.ssh/id_rsa): 
	Created directory '/home/deploy/.ssh'.
	Enter passphrase (empty for no passphrase): 
	Enter same passphrase again: 
	Your identification has been saved in /home/deploy/.ssh/id_rsa.
	Your public key has been saved in /home/deploy/.ssh/id_rsa.pub.
	The key fingerprint is:
	SHA256:YWxLvefkQjLLej1qo0RbDprhIlmsv2gVu5agb2/df/k jim-zong@17llife.com
	The key's randomart image is:
	+---[RSA 2048]----+
	|                 |
	|       . .       |
	|        * .      |
	| . .   + o .     |
	|  o + o S o o    |
	| = + = * = =     |
	|= + *.o.+.. o.   |
	|.=.=....= o.o    |
	|oo=+. o+.+.o .E  |
	+----[SHA256]-----+
	



###把生成的公钥加到github上去

```
/home/deploy/.ssh/id_rsa.pub 
```











	deploy@VM-155-46-ubuntu:~/site/mySite$ git branch -a
	fatal: Not a git repository (or any of the parent directories): .git
	deploy@VM-155-46-ubuntu:~/site/mySite$ git init 
	Initialized empty Git repository in /home/deploy/site/mySite/.git/
	deploy@VM-155-46-ubuntu:~/site/mySite$ git add . 
	deploy@VM-155-46-ubuntu:~/site/mySite$ git commit -m "first post"
	[master (root-commit) be78143] first post
	 8 files changed, 200 insertions(+)
	 create mode 100644 .gitignore
	 create mode 100644 404.html
	 create mode 100644 Gemfile
	 create mode 100644 Gemfile.lock
	 create mode 100644 _config.yml
	 create mode 100644 _posts/2017-06-22-welcome-to-jekyll.markdown
	 create mode 100644 about.md
	 create mode 100644 index.md
	deploy@VM-155-46-ubuntu:~/site/mySite$ git remote add origin https://github.com/jim-zong/jim-zong.github.io.git
	deploy@VM-155-46-ubuntu:~/site/mySite$ git branch -a
	* master
	deploy@VM-155-46-ubuntu:~/site/mySite$ git push origin master
	Username for 'https://github.com': ^C
	deploy@VM-155-46-ubuntu:~/site/mySite$ git remote -v
	origin	https://github.com/jim-zong/jim-zong.github.io.git (fetch)
	origin	https://github.com/jim-zong/jim-zong.github.io.git (push)
	deploy@VM-155-46-ubuntu:~/site/mySite$ git remote rm origin 
	deploy@VM-155-46-ubuntu:~/site/mySite$ git remote -v
	deploy@VM-155-46-ubuntu:~/site/mySite$ git remote add origin git@github.com:jim-zong/jim-zong.github.io.git
	deploy@VM-155-46-ubuntu:~/site/mySite$ git push origin master
	To git@github.com:jim-zong/jim-zong.github.io.git
	 ! [rejected]        master -> master (fetch first)
	error: failed to push some refs to 'git@github.com:jim-zong/jim-zong.github.io.git'
	hint: Updates were rejected because the remote contains work that you do
	hint: not have locally. This is usually caused by another repository pushing
	hint: to the same ref. You may want to first integrate the remote changes
	hint: (e.g., 'git pull ...') before pushing again.
	hint: See the 'Note about fast-forwards' in 'git push --help' for details.
	deploy@VM-155-46-ubuntu:~/site/mySite$ git pull
	warning: no common commits
	remote: Counting objects: 3, done.
	remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 3
	Unpacking objects: 100% (3/3), done.
	From github.com:jim-zong/jim-zong.github.io
	 * [new branch]      master     -> origin/master
	There is no tracking information for the current branch.
	Please specify which branch you want to merge with.
	See git-pull(1) for details.
	
	    git pull <remote> <branch>
	
	If you wish to set tracking information for this branch you can do so with:
	
	    git branch --set-upstream-to=origin/<branch> master
	
	deploy@VM-155-46-ubuntu:~/site/mySite$ git pull origin master
	From github.com:jim-zong/jim-zong.github.io
	 * branch            master     -> FETCH_HEAD
	Merge made by the 'recursive' strategy.
	 README.md | 1 +
	 1 file changed, 1 insertion(+)
	 create mode 100644 README.md
	deploy@VM-155-46-ubuntu:~/site/mySite$ git diff
	deploy@VM-155-46-ubuntu:~/site/mySite$ git push origin master
	Counting objects: 13, done.
	Compressing objects: 100% (12/12), done.
	Writing objects: 100% (13/13), 3.97 KiB | 0 bytes/s, done.
	Total 13 (delta 1), reused 0 (delta 0)
	remote: Resolving deltas: 100% (1/1), done.
	To git@github.com:jim-zong/jim-zong.github.io.git
	   e4c480d..3cab45c  master -> master
	deploy@VM-155-46-ubuntu:~/site/mySite$ ll
	total 56
	drwxrwxr-x 6 deploy deploy 4096 Jun 22 17:42 ./
	drwxrwxr-x 4 deploy deploy 4096 Jun 22 17:27 ../
	drwxrwxr-x 8 deploy deploy 4096 Jun 22 17:42 .git/
	-rw-r--r-- 1 deploy deploy   35 Jun 22 15:41 .gitignore
	drwxrwxr-x 4 deploy deploy 4096 Jun 22 15:46 .sass-cache/
	-rw-r--r-- 1 deploy deploy  398 Jun 22 15:41 404.html
	-rw-rw-r-- 1 deploy deploy  935 Jun 22 15:41 Gemfile
	-rw-rw-r-- 1 deploy deploy 1154 Jun 22 15:44 Gemfile.lock
	-rw-rw-r-- 1 deploy deploy   20 Jun 22 17:42 README.md
	-rw-r--r-- 1 deploy deploy 1648 Jun 22 15:41 _config.yml
	drwxrwxr-x 2 deploy deploy 4096 Jun 22 15:41 _posts/
	drwxrwxr-x 5 deploy deploy 4096 Jun 22 17:42 _site/
	-rw-r--r-- 1 deploy deploy  539 Jun 22 15:41 about.md
	-rw-r--r-- 1 deploy deploy  213 Jun 22 15:41 index.md
	deploy@VM-155-46-ubuntu:~/site/mySite$ vim cname
	deploy@VM-155-46-ubuntu:~/site/mySite$ ll
	total 60
	drwxrwxr-x 6 deploy deploy 4096 Jun 22 17:52 ./
	drwxrwxr-x 4 deploy deploy 4096 Jun 22 17:27 ../
	drwxrwxr-x 8 deploy deploy 4096 Jun 22 17:42 .git/
	-rw-r--r-- 1 deploy deploy   35 Jun 22 15:41 .gitignore
	drwxrwxr-x 4 deploy deploy 4096 Jun 22 15:46 .sass-cache/
	-rw-r--r-- 1 deploy deploy  398 Jun 22 15:41 404.html
	-rw-rw-r-- 1 deploy deploy  935 Jun 22 15:41 Gemfile
	-rw-rw-r-- 1 deploy deploy 1154 Jun 22 15:44 Gemfile.lock
	-rw-rw-r-- 1 deploy deploy   20 Jun 22 17:42 README.md
	-rw-r--r-- 1 deploy deploy 1648 Jun 22 15:41 _config.yml
	drwxrwxr-x 2 deploy deploy 4096 Jun 22 15:41 _posts/
	drwxrwxr-x 5 deploy deploy 4096 Jun 22 17:52 _site/
	-rw-r--r-- 1 deploy deploy  539 Jun 22 15:41 about.md
	-rw-rw-r-- 1 deploy deploy   17 Jun 22 17:52 cname
	-rw-r--r-- 1 deploy deploy  213 Jun 22 15:41 index.md
	deploy@VM-155-46-ubuntu:~/site/mySite$ git branch -a
	* master
	  remotes/origin/master
	deploy@VM-155-46-ubuntu:~/site/mySite$ git add .
	deploy@VM-155-46-ubuntu:~/site/mySite$ git commit -m "zlz170621553"
	[master 529f295] zlz170621553
	 1 file changed, 1 insertion(+)
	 create mode 100644 cname
	deploy@VM-155-46-ubuntu:~/site/mySite$ git push origin master
	Counting objects: 3, done.
	Compressing objects: 100% (2/2), done.
	Writing objects: 100% (3/3), 278 bytes | 0 bytes/s, done.
	Total 3 (delta 1), reused 0 (delta 0)
	remote: Resolving deltas: 100% (1/1), completed with 1 local object.
	To git@github.com:jim-zong/jim-zong.github.io.git
	   3cab45c..529f295  master -> master

### 给github.io绑定域名

我们个人都喜欢用自己的独立的域名，所以给绑定个自己的域名，在ROOT目录下定义一个CNAME的文件，写上你个人的域名，然后去域名服务商管理后台去cname 一下你的域名

	deploy@VM-155-46-ubuntu:~/site/mySite$ vim cname
	deploy@VM-155-46-ubuntu:~/site/mySite$ cat CNAME 
	blog.17llife.com

    jim-zong.github.io



	deploy@VM-155-46-ubuntu:~/site/mySite$ git add .
	deploy@VM-155-46-ubuntu:~/site/mySite$ git commit -m "zlz1706221759"
	[master 31cc0db] zlz1706221759
	 1 file changed, 0 insertions(+), 0 deletions(-)
	 rename cname => CNAME (100%)
	deploy@VM-155-46-ubuntu:~/site/mySite$ git push origin master
	Counting objects: 2, done.
	Compressing objects: 100% (2/2), done.
	Writing objects: 100% (2/2), 253 bytes | 0 bytes/s, done.
	Total 2 (delta 1), reused 0 (delta 0)
	remote: Resolving deltas: 100% (1/1), completed with 1 local object.
	To git@github.com:jim-zong/jim-zong.github.io.git
	   529f295..31cc0db  master -> master



自此您就可以访问您的个人博客了！！