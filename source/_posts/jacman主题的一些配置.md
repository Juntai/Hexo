title: jacman主题的一些配置
date: 2015-06-20 16:47:55
categories: jacman
tags: jacman
description: 分享jacman主题的一些配置，和对[如何搭建一个独立博客——简明Github Pages与Hexo教程]的一些修改。
---
刚按照[如何搭建一个独立博客——简明Github Pages与Hexo教程](http://cnfeat.com/2014/05/10/2014-05-11-how-to-build-a-blog/)在github上建立了这个博客。
很久之前自己就尝试过在github上搭建博客，是基于jekyll的，做完之后感觉太丑漏了。最近又想起这件事，觉得自己有点蛇头蛇尾，所以趁着端午假期把博客正式建立起来。
用了大半天时间终于看着像那么回事了。在此感谢cnfeat陈素封博主！

本文主要包括两个部分：
* 配置：cnfeat把绝大部分工作都详尽地写出来了，剩下的多是些配置工作，我把我做的一些配置记录在本文；
* 教程的一些修改：可能是环境和软件版本的问题，教程中的个别地方需要稍作修改，也记录在本文。

# 配置jacman
## 公式
jacman支持公式编辑，我复制过来的模板中没有打开这个选项，只需要在配置文件中将其修改为可用即可。
在/Hexo/themes/jacman/_config.yml文件中修改：
> mathjax: True       #enable mathjax if true

## 评论
jacman支持多说和disqus评论，只需要在/Hexo/themes/jacman/_config.yml文件中的对应位置加上在多说或disqus申请到的short_name即可。
我使用的是多说，配置修改如下：
> duoshuo_shortname: juntaiblog   ## e.g. wuchong   your duoshuo short name.
> disqus_shortname:     ## e.g. wuchong   your disqus short name.

# 教程的一些修改
我是在win7-64bit上做的，下面是我安装的hexo版本信息：
![hexo版本信息](/images/hexo-version.png)

## 部署Hexo
第一次在浏览器中输入localhost:4000查看时，按照教程输入命令
``` blash
$ hexo g
$ hexo s
```
服务不启动，我采用的方法是输入如下命令：
``` blash
$ hexo g
$ npm install
$ hexo s
```
这样服务就启动了。并且以后不需要再增加npm install了。

## 使用与调试
部署到Github前需要配置_config.yml文件，若按按照教程配置type: github，部署的时候会报错。
> deploy:
> type: github
> repository: git@github.com:Juntai/juntai.github.io
> branch: master

搜索了一下，可能与hexo的版本有关系，需要做如下修改。
* 把type改为git：
> deploy:
> type: git
> repository: git@github.com:Juntai/juntai.github.io
> branch: master

* 部署命令也要做修改，在部署之前安装hexo-deployer-git：
``` blass
hexo clean
hexo generate
npm install hexo-deployer-git --save
hexo deploy
```