---
title: Hexo+Github 建站整理（二）：多终端同步和图床
date: 2022-01-19 10:00:17
categories: [Hexo,整理]
tags: [Hexo,整理]
description: 多终端同步，CDN个人图床
---
{% blockquote %}
本文内容并非原创，部分内容来源于官方文档和网络，可以理解为建站之后的个人总结和整理，便于自己以后查阅。
{% endblockquote %}

![起始页面](https://cdn.jsdelivr.net/gh/wwyyff123/CDN/Photos/blogs/hexo&github2.png)

# 多终端同步

{% note default %}
多终端同步工作有很多解决办法，比较简单的直接使用onedrive，百度网盘直接同步，这里使用github的分支来进行多终端同步。
{% endnote %}

用hexo d命令上传到github的文件并不是源文件，而是编译后生成的网页文件

![](https://cdn.jsdelivr.net/gh/wwyyff123/CDN/Photos/blogs/11github仓库.jpg)

可以发现这里上传的文件和本地目录下的.deploy_git文件夹里的内容是一致的  

![](https://cdn.jsdelivr.net/gh/wwyyff123/CDN/Photos/blogs/12本地目录.jpg)

而其他的源文件并没有上传到github，接下来利用github的分支管理，将源文件上传。


## 建立分支

在github下博客的仓库里新建一个分支，命名为hexo

![新建分支](https://cdn.jsdelivr.net/gh/wwyyff123/CDN/Photos/blogs/13新建分支.jpg)

在仓库的settings中，修改默认分支为hexo

![修改设置](https://cdn.jsdelivr.net/gh/wwyyff123/CDN/Photos/blogs/14修改设置.jpg)

然后在本地的任意目录下，打开终端，输入
```
git clone git@github.com:用户名/用户名.github.io.git
```

随后hexo分支被clone到了本地目录，打开克隆到本地的文件夹，将里面除了.git文件夹外的所有文件全部删除  
然后在之前博客的源文件里，除了.deploy_git文件夹，其余文件全部复制到刚才clone的文件夹里，以后这个clone的文件夹变成了博客的新文件夹。需要注意的是复制过来的源文件应该有一个.gitignore文件，作用是忽略一些不必要的文件，如果没有，需要自己新建一个并且在里面写上  

```
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
```
在博客新目录下终端运行
```
git add .
git commit -m "添加分支"
git push
```
这样源文件就上传到了hexo分支下，检查一下有没有上传成功

![检查分支](https://cdn.jsdelivr.net/gh/wwyyff123/CDN/Photos/blogs/15hexo分支jpg.jpg)

到这里分支内容处理结束  
## 其他电脑操作
在其他电脑上首先需要搭建环境（参考第一篇文章）
{% note default %}
1. 安装git，并设置好git全局邮箱和用户名，以及ssh key
2. 安装node.js
3. 安装hexo
{% endnote %}
之后在任意目录下，输入
```
git clone git@github.com:用户名/用户名.github.io.git
```
将hexo分支下的源文件clone到本地，然后进入到源文件目录内运行终端
```
npm install
npm install hexo-deployer-git --save
hexo g
hexo d
```
到这里生成部署就完成了，以后要写新日志直接在该目录下运行
```
hexo n "文章名"
```

# Github+jsDelivr建立CDN个人图床

{% blockquote %}
CDN的全称是Content Delivery Network，即内容分发网络。CDN是构建在现有网络基础之上的智能虚拟网络，依靠部署在各地的边缘服务器，通过中心平台的负载均衡、内容分发、调度等功能模块，使用户就近获取所需内容，降低网络拥塞，提高用户访问响应速度和命中率。CDN的关键技术主要有内容存储和分发技术。
{% endblockquote %}
以上总结，加快图片加载速度。
## 新建Github仓库

首先新建一个仓库专门用于保存图片

![建立仓库](https://cdn.jsdelivr.net/gh/wwyyff123/CDN/Photos/blogs/16建立图床仓库.jpg)

然后在本地任意目录（这里我和blog文件夹放在一起），终端运行
```
git clone git@github.com:用户名/images.git
```

## 上传资源
需要外链的图片资源（jsDelivr最大支持20M），复制到该目录下，然后终端运行
```
git add .
git commit -m "上传图片"
git push
```
将图片资源上传到github

## 发布仓库
点击Create a new release，发布

![发布](https://cdn.jsdelivr.net/gh/wwyyff123/CDN/Photos/blogs/17发布release.jpg)

自定义版本号描述信息

![自定义](https://cdn.jsdelivr.net/gh/wwyyff123/CDN/Photos/blogs/18发布.jpg)

## 通过jsDelivr引用资源
https://cdn.jsdelivr.net/gh/用户名/仓库名@版本号/文件路径
如：
```
https://cdn.jsdelivr.net/gh/wwyyff123/CDN/Photos/blogs/hexo&github.png
```
版本号不是必须的，不加版本号将直接引用最新资源，其他详细的引用方式可以参考
[jsDelivr官方说明](https://www.jsdelivr.com/?docs=gh)
github的仓库有1G的容量限制，如果满了还可以新建一个仓库。








# 参考文章

{% note info %}
1、[文档 | Hexo](https://hexo.io/zh-cn/docs/)

2、[hexo史上最全搭建教程](https://blog.csdn.net/sinat_37781304/article/details/82729029?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522164145286816780271598619%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=164145286816780271598619&biz_id=0)

3、[免费CDN：jsDelivr+Github 使用方法](https://zhuanlan.zhihu.com/p/76951130)
{% endnote %}