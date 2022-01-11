---
title: Hexo+Github 建站整理（一）：搭建与部署
date: 2022-01-11 14:07:17
categories: [Hexo,整理]
tags: [Hexo,整理]
description: 整理一下从0到1的过程，记录一下遇到的坑
---
{% blockquote %}
本文内容并非原创，部分内容来源于官方文档和网络，可以理解为建站之后的个人总结和整理，便于自己以后查阅。
{% endblockquote %}

![Hexo$Github](https://cdn.jsdelivr.net/gh/wwyyff123/CDN/Photos/blogs/hexo&github.png)

# Hexo简介
{% note info %}
Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 [Markdown](http://daringfireball.net/projects/markdown/)（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。
{% endnote %}

# Hexo的搭建与部署
{% note default %}
此部分为Hexo的搭建、如何部署到Github以及个人域名的绑定
{% endnote %}

## 获得域名（可跳过）
申请个人域名可以让自己的博客更加个性化，常见的域名有com,cn,net...等等，申请域名的地方有很多，比如 [阿里云](https://wanwang.aliyun.com/domain/)和[腾讯云](https://buy.cloud.tencent.com/domain)，我这里使用的是腾讯云，具体步骤参考具体域名购买网址就可以了。这里是整个搭建过程里唯一需要氪金的地方，当然也可以不购买域名，直接使用github的域名就可以了。  

## Github创建仓库
首先登陆Github账号，点击New repository创建新仓库，仓库名应该为: 用户名/github.io，这个是固定写法，用户名必须与Github的账号名一致。
![创建仓库](https://cdn.jsdelivr.net/gh/wwyyff123/CDN/Photos/blogs/1创建仓库.jpg)
## 安装Git
{% note default %}
Git是目前世界上最先进的分布式版本控制系统。
{% endnote %}
点击[Git](https://git-scm.com/download/win)选择对应版本下载，一路下一步安装即可。安装好后终端输入
   
   ```
   git --version //检测git是否安装成功
   ```
   
   如果能查看到版本说明安装成功。
   ![Git版本](https://cdn.jsdelivr.net/gh/wwyyff123/CDN/Photos/blogs/2git版本.jpg)
   
安装后绑定Github账号，打开终端输入:

```
git config --global user.name "GitHub用户名"
git config --global user.email "GitHub注册邮箱"
```

生成ssh密钥：

   ```
   ssh-keygen -t rsa -C "GitHub注册邮箱"
   ```

   直接回车3次，然后找到生成的.ssh文件夹中id_rsa.pub密钥，将里面的内容全部复制
   ![ssh密钥](https://cdn.jsdelivr.net/gh/wwyyff123/CDN/Photos/blogs/3ssh密钥.jpg)

   打开[GitHub_Settings_keys](https://link.zhihu.com/?target=https%3A//github.com/settings/keys)页面，点击New SSH key，Title随便写，Key填入之前复制的内容，最后点击Add SSH key
   ![sshkey](https://cdn.jsdelivr.net/gh/wwyyff123/CDN/Photos/blogs/4newsshkey.jpg)
   
   最后在终端中输入
   
   ```
   ssh git@github.com
   ```
   
   显示successfully说明成功了
   ![sshkey](https://cdn.jsdelivr.net/gh/wwyyff123/CDN/Photos/blogs/5检验ssh.jpg)
## 安装Node.js

Hexo是基于Node.js编写的，所以需要在[Download | Node.js](https://nodejs.org/zh-cn/download/)下载安装，一直下一步即可，Node.js会包含环境变量和npm的安装，安装后终端输入

```
node -v //检测Node.js是否安装成功
npm -v //检测npm是否安装成功
```

显示了版本号说明安装成功了

![检查安装](https://cdn.jsdelivr.net/gh/wwyyff123/CDN/Photos/blogs/6检查node.jpg)

## 安装Hexo

在电脑上创建blog的文件夹，可以放在onedrive的文件夹里方便同步，然后文件夹里按住shift右键空白点击在此处打开pwershell，输入

```
npm install -g hexo-cli  //安装hexo
```

命令完成后依次输入

```
hexo init myblog  //初始化
cd myblog  //进入myblog文件夹
npm install 
```

完成后，myblog里应该会有以下文件

```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

然后输入

```
hexo g  //生成页面
hexo s  //启动服务预览
```
随后浏览器输入localhost:4000就可以看到了
![起始页面](https://cdn.jsdelivr.net/gh/wwyyff123/CDN/Photos/blogs/7起始页面.jpg)

{% note danger %}
如果无法打开localhost:4000，参考以下：
1、4000端口被其他程序占用：上一步里输入hexo s -p 5000将端口号改为5000，然后浏览器输入localhost:5000
2、IIS服务未启用：控制面板-启用或关闭Windows功能，勾选internet information services确定就行了
{% endnote %}

## 将Hexo部署到Github

打开博客文件夹里的_config.yml，翻到最后修改相关信息

```
deploy:
  type: git
  repo: https://github.com/用户名/用户名.github.io.git
  branch: master
```

然后安装deploy-git，在博客文件夹里打开终端，输入

```
npm install hexo-deployer-git --save
```

最后输入

```
hexo clean  //清理
hexo g  //生成页面
hexo d //部署页面
```

最后浏览器输入 <用户名.github.io>就可以访问了

## 绑定域名

第一步如果购买了自己的域名，可以在此时将博客绑定。登陆腾讯云的域名管理后台，点击解析

![解析](https://cdn.jsdelivr.net/gh/wwyyff123/CDN/Photos/blogs/8解析.jpg)

添加以下解析记录

![解析2](https://cdn.jsdelivr.net/gh/wwyyff123/CDN/Photos/blogs/9解析2.jpg)

其中第一条记录值的ip需要自己ping xxxx.github.io获得，第二条记录填xxxx.github.io

登陆Github，进入之前创建的仓库，点击setting-pages，在custom domain处输入要绑定的域名，点击save保存

![绑定](https://cdn.jsdelivr.net/gh/wwyyff123/CDN/Photos/blogs/10绑定.jpg)

进入博客文件夹/source目录下，创建记事本文件，输入自己的域名，然后保存命名为CNAME（不带后缀名）

最后在博客文件夹目录里打开终端，输入

```
hexo clean
hexo g
hexo d
```

就可以通过购买的域名访问了。

到这里，博客的搭建基本完成。

## 发布文章

在博客文件夹目录打开终端输入

```
hexo n "文章名"
```

然后在博客文件夹/source/_post下可以看到生成的markdown文件，打开就可以编辑了，写完后再输入以下代码就发布成功了

```
hexo g
hexo d
```

关于markdown语法，可以参考[Markdown 教程 | 菜鸟教程 (runoob.com)](https://www.runoob.com/markdown/md-tutorial.html)，编写工具可以用Typora或者visual studio code

# 参考文章

{% note info %}
1、[文档 | Hexo](https://hexo.io/zh-cn/docs/)

2、[GitHub+Hexo 搭建个人网站详细教程 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/26625249)
{% endnote %}