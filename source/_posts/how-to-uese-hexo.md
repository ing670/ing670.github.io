---
title: 如何使用hexo搭建自己的博客
date: 2017-05-24 17:21:49
categories:
- 教程
tags:
- hexo
- blog
- node
- js
---
我的这个站点就是使用[Hexo](https://hexo.io/) 配合[github](https://github.com/)提供的gitpage搭建的。
利用gitpage搭建博客有常用的有两种方式[jeklly](http://jekyllrb.com/)和[Hexo](https://hexo.io/)我个人对js比较熟悉，
所以选择[Hexo](https://hexo.io/)  
现在我分享这种建立个人博客的方法。

### 环境准备
- [node](https://nodejs.org/en/)
- [git](https://git-scm.com/)
- [Hexo](https://hexo.io/)  

不同操作系统，安装教程从官方网站都能找到。(当前系统为mac系统)  

### 安装node版本管理器
我推荐使用[nvm](http://nvm.sh)来安装。其中node版本切换那是相当的爽
### 下载nvm
``` bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
```
Or
``` bash
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
```
此时nvm是安装到`~/.nvm`下。
### 配置nvm环境变量
然后把下面这个脚本添加到你的环境变量里面去(`~/.bash_profile`)
``` bash
vim ~/.bash_profile
```
黏贴下面这段代码到文件最末尾

``` bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" # This loads nvm
```
执行
``` bash
source ~/.bash_profile
```
### 验证nvm安装是否成功
``` bash
nvm
```
看是否输出一堆帮助信息
### 安装node
选择node版本
``` bash
nvm ls-remote
```
然后找个相对高版本安装即可
``` bash
nvm instal v7.10.0
```
### 验证node是否安装成功
``` bash
node -v
```
正确输出我们选择node的版本
### 安装git
看这篇官方教程已经够了
[git安装官方教程](https://git-scm.com/book/zh/v1/%E8%B5%B7%E6%AD%A5-%E5%AE%89%E8%A3%85-Git)
### 配置gitpage
- 浏览器打开 https://github.com/ing670 这里以我的github账号为例
- 点击 `repositories`这个tab，然后点击左边new 按钮新建一个repository，注意名字必须`your_user_name.github.io`，比如我的
就是`ing670.github.io`
- 新建完成，如果你添加了README，直接输入your_user_name.github.io 就能访问到你的README
- 配置github ssh[点击这里](http://www.cnblogs.com/yehui-mmd/p/5962254.html)，图文并茂

### Hexo安装

``` bash
npm install hexo-cli -g
```
### Hexo创建我们的blog
``` bash
hexo init blog
cd blog
npm install
hexo server
```
此时，在浏览器输入http://localhost:4000 就能看到效果了
如果发生没反应的情况，使用`hexo s -p 9999`换个端口试一试

### 新建一篇文章
有两种方式：
- 在`source/_posts` 新建一个以md结尾的文件，把hello-world.md格式拷贝过来就行
- 使用命令行
``` bash
cd  your_blog_path
hexo new post 'post_name'
```
### 配置发布地址
``` bash
vim  your_blog_path/_config.yml
```
找到deploy 添加如下信息
```
deploy:
  type: git
  repo: git@github.com:ing670/ing670.github.io.git(这里替换成你的git地址)
  branch: master
```
注意：配置这个前提是先配置好[github ssh](#配置gitpage)
### 发布文章到github
``` bash
hexo g -d
```
详细信息: [Deployment](https://hexo.io/docs/deployment.html)
### 也许你还需要一个主题
这里有非常多的主题[点我](https://hexo.io/themes/)
我的主题是：[NexT](https://github.com/iissnan/hexo-theme-next)
使用主题有个注意的点：在_config.yml的theme对应的名称必须是在themes文件下的文件夹名称。
### 总结
其实对于有点基础的人来说，这个东西不用多久就能搞定了。这是我的第一篇技术文章。希望可以帮助到有需要的人
