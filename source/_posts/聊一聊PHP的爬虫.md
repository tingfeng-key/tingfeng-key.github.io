---
title: 聊一聊PHP的爬虫
date: 2017-11-18 15:46:13
tags: [爬虫, php]
---
之前做过一个基于laravel的视频爬虫项目，项目地址：[https://github.com/tingfeng-key/qqVideo](https://github.com/tingfeng-key/qqVideo)
<!-- more -->
## 爬虫基本原理
做过爬虫项目的同学对爬虫的原理自然不会陌生，为了照顾没有做过此类项目的朋友，请原谅我啰嗦几句；
#### 原理：
	获取目标网页的内容（也可能是json、xml等链接的数据
	---> 分析目标网页的内容
	---> 提取需要的数据
	---> 保存数据
以上步骤完成是不是就算结束了呢？可以算结束了，如果不考虑及时更新或者可以手动更新的情况的话。

比如你爬取的是优酷视频《猎场》的视频信息（标题、演员、专辑更新等等），是不是《猎场》更新一集你就手动更新一集呢？

作为一个合格的程序员，我们更加希望的是把这些工作都交给计算机来完成，毕竟我们的时间还是很宝贵的，那么怎么做？

#### 定时任务
定时任务指的是计算机在我们指定的时间去执行一些事情，关于定时任务这里也不多介绍，具体的请度娘或goole。

那么定时任务能帮我们做什么？

__举个栗子：__
比如我在09点出门，我想计算机下载完电影后自动关机，可能需要一小时的时间，那么我们可以设置电脑在10:10时自动关机。（说了这么多，那么定时任务在爬虫里能做什么呢？）

定时任务可以帮助我们自动执行脚本，比如linux计算机里，执行shell脚本、js脚本、php脚本等等。

我们编写好程序后就可以帮执行采集程序的命令写入一个shell脚本中或者直接放在cronetab中。

### 说说我的采集视频项目
#### 关于此项目（废话还是要啰嗦）
这个项目是基于laraval框架的PHP项目，使用了phpquery包做dom分析，使用redis做队列，mysql做数据储存，其中使用到laravel的job（事件+监听器）和command（命令行artisan），redis也可以用mysql代替。

#### 环境依赖
php5.6.27+mysql5.6+supervisor
#### 安装qqVideo
1. 首先先`git clone https://github.com/tingfeng-key/qqVideo.git`。（关于git的安装和使用请自行度娘）
2. 命令行里执行： `composer install`，（关于composer同上）
3. 配置`.env`文件。（具体请参考laravel的安装和配置）
#### 使用
1. 执行`php artisan list` , 查看可用命令行（依赖php的全局变量）
2. 执行`php artisan video:qq` 采集腾讯视频的信息
3. 执行`php artisan video:youku` 采集优酷视频的信息

#### 添加到定时任务
```shell
crontab -e
php 项目路径/artisan video:qq
php 项目路径/artisan video:youku
```
#### supervisor 配置
请参考laravel文档中队列的supervisor配置