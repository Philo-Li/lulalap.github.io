---
title: 已解决 AttributeError:'NoneType' object has no attribute 'find_all'
date: 2018-11-05 13:00:55
tags: 爬虫
categories: 日常折腾
---
### 问题描述：
当像这样使用 BeautifulSoup 时，会遇到报错信息：** AttributeError: 'NoneType' object has no attribute 'find_all' **
<!--more-->
```bash
soup = BeautifulSoup(html,features='lxml')
content = soup.find('article', {"class":'post post-type-normal'})
d_body = content.find_all('li')
```
### 解决办法：
查了很多博客，但是都没说到点上，最后找到了。其实是 soup.find() 里的参数不对，没有匹配到任何信息，导致上一步得到的 content 是空的，因此后面会报错。调整了一下参数，完美运行。
