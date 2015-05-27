---
layout: default
title:  "Content Provider(一)"
date:   2015-01-15 12:34:30
categories: Android
tags: ContentProvider
---

# Content Provider(一)
 

ContentProvider用于访问结构化数据，他们在压缩数据的同时提供了保护数据安全的机制，并且提供了标准化接口用于在一个进程在另一个进程中进行数据访问。

在访问contentProvider提供的数据时，需要使用ContentResolver对象作为客户端来访问创建了ContentProvider对象的应用程序，由contentProvider执行访问要求并返回结果。

Android提供音频，视频，图象，联系人等的ContentProvider，在android.provider文档中看到有关介绍。

接下来主要介绍这几个方面：

1. **[ContentProvider基础](http://qwe3243002.github.io/jekyll/update/2015/01/15/Content%20Provider(%E4%BA%8C).html)**

	    如何访问表格中的数据

2. **创建CotentProvider**

	    如何创建自己应用的ContentProvider
3. **日历ContentProvider**

	    访问日历提供数据
4. **联系人ContentProvider**

	    访问联系人提供数据