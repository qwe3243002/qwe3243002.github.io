---
layout: post
title:  "Animation and Graphics(二)"
date:   2015-01-12 16:38:30
categories: jekyll update
---
# 属性动画 #
属性动画的强大在于几乎能使所有东西能运动起来。如果需要指定某个对象运动，你需要通过指定你想修改的对象属性，如坐标，运动时间，最大值，最小值等。

通过属性动画你可以指定如下这些属性：

- Duration：动画持续时间，默认300ms
- Time interpolation：时间插值函数，规定动画持续时间中物体的加速减速。
- Repeat count and behavior：可以指定是否需要重复以及重复次数，并且指定重复方式是逆序播放还是顺序播放。
- Animator sets：将一组动画放入逻辑集合中，并且能规定是同时播放，还是依次播放，还是延时播放。
- Frame refresh delay：规定每一帧刷新的间隔时间，默认10ms，但是由于系统的繁忙程度和服务速度有可能会有所偏差。

