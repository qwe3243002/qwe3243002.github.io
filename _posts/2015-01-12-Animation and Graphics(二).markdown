---
layout: post
title:  "Animation and Graphics(二)"
date:   2015-01-12 16:38:30
categories: jekyll update
---
# **属性动画** #
属性动画的强大在于几乎能使所有东西能运动起来。如果需要指定某个对象运动，你需要通过指定你想修改的对象属性，如坐标，运动时间，最大值，最小值等。

通过属性动画你可以指定如下这些属性：

- Duration：动画持续时间，默认300ms。
- Time interpolation：时间插值函数，规定动画持续时间中物体的加速减速。
- Repeat count and behavior：可以指定是否需要重复以及重复次数，并且指定重复方式是逆序播放还是顺序播放。
- Animator sets：将一组动画放入逻辑集合中，并且能规定是同时播放，还是依次播放，还是延时播放。
- Frame refresh delay：规定每一帧刷新的间隔时间，默认10ms，但是由于系统的繁忙程度和服务速度有可能会有所偏差。

## **属性动画是如何工作的** ##
首先看一个小例子。我们假设操作一个对象的x属性，即表示让他在水平方向上移动。动画持续时间是40ms，并且移动距离为40px。也就是说在默认为10ms的刷新时间中，每隔10ms该对象移动10px。在40ms持续时间过去后动画停止，并且该对象水平位移40px。在这个例子中，该对象是匀速行驶，即水平速度为常量。

![](http://developer.android.com/images/animation/animation-linear.png)

在看一个例子，我们只改变该对象水平运动为非匀速行驶，是该对象在前半段时间内加速行驶，后半段时间内减速行驶，这样结果如下：

![](http://developer.android.com/images/animation/animation-nonlinear.png)

最后，我们详细分析一下属性动画的计算过程。

![](http://developer.android.com/images/animation/valueanimator.png)

ValueAnimator对象主要跟踪动画校准，比如该动画执行了多少时间，当前属性的值等。
ValueAnimator封装了TimeInterpolator属性，用于指定使用哪种时间插值函数。TypeEvalutor指定在动画执行过程中如何计算属性值。比如在第二个例子中TimeInterploator指定为AccelerateDecelerateInterpolator，而TypeEvalutor指定为IntEvalutor。

在开始一个动画前，我们必须创建一个ValueAnimator，并且给定某一动画属性的开始值和结束值以及持续时间。当你调用start()方法开始动画时，ValueAnimator会计算已经执行时间的百分比（0~1），用来判断是否该动画已经完成。当ValueAnimator计算完一次百分比之后，会调用TimeInterpolator对象去执行一个插值函数去获得当前时间的加速度值。在插值函数计算完后，ValueAnimator就会调用TypeEvalutor对象进行计算得到最后的属性值。
在谷歌API例子的**com.example.android.apis.animation**包中提供了多个关于如何使用属性动画的案例。

## **属性动画与视图动画的区别** ##
