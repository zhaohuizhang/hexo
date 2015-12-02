title: Python Scrapy+MongoDB
tags:
  - crawler
  - mongodb
  - Scrapy
id: 85
categories:
  - Python
date: 2015-04-26 17:12:11
---

用户想用一个Python程序从StackOverflow抓取数据，获取新的问题（问题标题和URL），抓取的数据应当存入MongoDB。

在开始抓取工作前，一定要先查看该网站的使用/服务条款，要尊重robots.txt文件。抓取行为应当遵守道德，不要在很短的时间内发起大量请求，从而导致网站遭受泛洪攻击。

本文安装运行环境：windows 7 Enterprise  64bit

安装：

[Scrapy](http://scrapy.org/)，[PyMongo ](https://api.mongodb.org/python/current/)，[MongoDB](https://www.mongodb.org/)，[Pywin32](http://sourceforge.net/projects/pywin32/files/pywin32/)
<pre>pip install Scrapy
pip install pymongo
scrapy startproject stack
Problem：NO module name service_identity
solution:pip install service_identity</pre>
[Github](https://github.com/zhaohuizhang/ScrapySpider)