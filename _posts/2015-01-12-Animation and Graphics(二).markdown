---
layout: default
title:  "Animation and Graphics(二)"
date:   2015-01-12 16:38:30
categories: Android
tags: 动画
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

## **属性动画与视图动画的区别** #

    - 视图动画只是对于视图对象提供了强大的功能，而对于非试图组件对象则需要你自己写代码来实现。视图动画还有一些局限在于他只能影响视图组件一些方面比如缩放，旋转，但不会改变其背景颜色。
    - 视图动画的另一个缺点就是他只是指定组件被画的位置，而不是组件实际位置
    - 属性动画就完全没有视图动画的缺点，他不但可以操纵任何对象，并且会实际修改该对象的属性。
    - 然而视图动画的创建需要的时间更少，所需的代码量也会减少。所以你需要根据情况选择合适的动画系统。

## **API概述** ##
属性动画的API位于android.animation,而在android.view.animation中还有大量的插值函数类。

**Animator类提供了创建动画的基本数据结构。由于提供的功能不够全面，通常情况下不需要直接使用该对象。**

    **ValueAnimator**
    	- 属性动画的主要时序引擎并计算动画中的属性值。
    	- 含有计算属性值的核心方法，包括每个动画的时序细节，每个动画是否重复，监听更新事件，以及自定义计算类型
    **ObjectAnimator**
    	- ValueAnimator的子类，允许你设置一个目标对象以及目标属性来进行动画。
    	- 比ValueAnimator使用更简单，但是也有一些限制，如目标对象上必须定义一些特定访问方法。
    **AnimatorSet**
    	- 提供播放一组动画的机制，你可以规定是同时播放，顺序播放还是延时播放。

**Evaluators**

    - **IntEvaluator**
    	- 默认Int类型的计算器
    - **FloatEvaluator**
    	- 默认Float类型的计算器
    - **ArgbEvaluator**
    	- 默认计算Color属性的计算器，值为16进制
    - **TypeEvaluator**
    	- 计算器接口，可以用于实现自定义类型的计算

**Interpolators**

    - **AccelerateDecelerateInterpolator**
    	- 先加速后减速，其变化率开始和结束缓慢而中间加速
    - **AccelerateInterpolator**
    	- 加速，其变化率慢慢提高
    - **AnticipateInterpolator**
    	- 开始时先向后，然后在先前甩
    - **AnticipateOvershootInterpolator**
    	- 开始时先向后，然后向前甩，冲过目标对象，最后返回目标对象
    - **BounceInterpolator**
    	- 表示在动画结束时再弹一下
    - **CycleInterpolator**
    	- 指定动画重复次数
    - **DecelerateInterpolator**
    	- 减速，其变化率逐渐降低
    - **LinearInterpolator**
    	- 匀速行驶
    - **OverShootInterpolator**
    	- 开始时向前甩动，冲过目标对象，最后返回目标对象
    - **TimeInterpolator**
    	- 接口，可以自己定制插值函数

## **ValueAnimator动画** ##

ValueAnimator类通过指定类型来创建ValueAnimator对象及内部计算方式，通过ofInt()，ofFloat(),ofObject就可以创建ValueAnimator对象。

    ValueAnimator animator = ValueAnimator.ofFloat(0f,1f);
    animator.setDuration(2000);
    animator.start();

在start()调用过后ValueAnimator便开始计算消耗时间百分比，你也可以自定义类型来创建ValueAnimator对象。

    ValueAnimator animation = ValueAnimator.ofObject(new MyTypeEvaluator(), startPropertyValue, endPropertyValue);
    animation.setDuration(1000);
    animation.start();

按照以上代码ValueAnimator开始通过MyTypeEvaluator计算在startPropertyValue与endPropertyValue之间的值。

## **ObjectAnimator动画** ##
ObjectAnimator是ValueAnimator的子类并且集合了时序引擎以及强有力的对目标对象属性值得计算。这样我们就不需要实现ValueAnimator.AnimatorUpdateListener接口了，因为该属性值会自动进行更改。

    ObjectAnimator objectAnimator = ObjectAnimator.ofFloat(foo,"alpha",0f,1f);
    objectAnimator.setDuration(2000);
    objectAnimator.start();

为了使ObjectAnimator更新的属性值正确，要必须这样：

    - 目标对象的属性必须set方法，并且遵从`set<propertyName>()`格式
    - 如果你在使用ObjectAnimator的工厂方法来创建对象时，在传入values...参数是，只传入一个值，那么该值将作为结束值，这样的话，你必须对于该属性有一个get方法，并遵从`get<propertyName>()`格式
    - get方法与set方法的参数类型以及返回值类型必须一致
    - 对于属性及对象动画，你可能需要调用`invalidate()`来强制重新绘制视图，一般你会在`onAnimationUpdate()`方法中调用。但是对于视图的属性，你就不需要再调用该方法了，因为该方法会有系统在恰当的时间调用。

## **采用AnimationSet设计动画** ##

采用AnimatorSet来组合动画或者组合AnimatorSet来播放动画
{% highlight java %}

    AnimatorSet bouncer = new AnimatorSet();
    bouncer.play(bounceAnim).before(squashAnim1);
    bouncer.play(squashAnim1).with(squashAnim2);
    boncer.play(squashAnim1).with(stretchAnim1);
    bouncer.play(squashAnim1).with(stretchAnim2);
    bouncer.play(bounceBackAnim).after(stretchAnim2);
    ValueAnimator fadeAnim = ObjectAnimator.ofFloat(new Ball,"alpha",1f,0f);
    fadeAnim.setDuration(250);
    animatorSet.play(bouncer).before(fadeAnim);
    animatorSet.start();
    
{% endhighlight %}

## **动画监听器** ##
    - Animator.AnimatorListener
    	- onAnimationStart()——在动画开始时调用
    	- onAnimationEnd()——在动画结束时调用
    	- onAnimationRepeat()——在动画要重复自身时调用
    	- onAnimationCancel()——在动画被取消时调用
    - ValueAnimator.AnimatorUpdateListener
    	- onAnimationUpdate()——在动画每一帧刷新时调用

如果不想实现AnimatorUpdateListener接口的话，可以继承AnimatorListenerAdapter类，从而只是重写你想调用的方法。

    ValueAnimator fadeAnim = ObjectAnimator.ofFloat(newBalls,"alpha",0f,1f);
    fadeAnim.setDuration(3000);
    fadeAnim.addListener(new AnimatorListenerAdapter() {
	    @Override
	    public void onAnimationEnd(Animator animation) {
	    	balls.remove(((ObjectAnimator)animation).getTarget());
	    }
    });


## **动画布局更改ViewGroup** ##
属性动画也提供了在布局更改时的过渡动画，通过调用setAnimator()方法可以定义在LayoutTransition中的动画

LayoutTransition中的常量：

    - APPERING：指示已经出现在布局中的组件的动画
    - CHANGE_APPERING:指示对于已经出现在布局中的组件由于新的组件出现而产生的动画
    - DISAPPERING：指示组件正在消失在布局中执行的动画
- CHANGE_DISAPPERING:指示由于组件消失中，依旧存在布局中的组件的动画

根据这四种事件类型既可以自定制过渡动画也可以使用系统默认的动画效果。

在样例LayoutAnimations中展示了如何定制布局过渡动画。LayoutAnimationsByDefault以及layout_animations_by_default.xml展示了如何使用系统提供的动画，对于开发者重要的是设置android:animateLayoutchanges="true"，如下

    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/white"
    android:animateLayoutChanges="true"/>

## **动画视图** ##
从Android3.0开始加入了新的属性来增强属性动画：

    - translationX和translationY：表示在父容器中的坐标
    - scaleX和scaleY：控制组件在2D中围绕中心点的缩放
    - rotation，rotationX，rotationY：控制组件围绕中心点在2D/3D中旋转
    - pivotX和pivotY：用于指定中心点的位置，默认情况下以组件中心为中心点
    - x和y：描述组件在容器中的实际位置.x=left+translationX
    - alpha:透明度，默认1，0表示不显示

## **动画中的ViewPropertyAnimator** ##

ViewPropertyAnimator提供一种简单的方式只用一个Animator对象就可以实现组件同时改变好几个属性

Multiple ObjectAnimator objects

    ObjectAnimator animX = ObjectAnimator.ofFloat(myView, "x", 50f);
    ObjectAnimator animY = ObjectAnimator.ofFloat(myView, "y", 100f);
    AnimatorSet animSetXY = new AnimatorSet();
    animSetXY.playTogether(animX, animY);
    animSetXY.start();

One ObjectAnimator

    PropertyValuesHolder pvhX = PropertyValuesHolder.ofFloat("x", 50f);
    PropertyValuesHolder pvhY = PropertyValuesHolder.ofFloat("y", 100f);
    ObjectAnimator.ofPropertyValuesHolder(myView, pvhX, pvyY).start();

ViewPropertyAnimator

    myView.animate().x(50f).y(100f);

在xml文件中声明动画
属性动画的xml文件统一保存在res/animator文件夹中
接着需要加载该xml文件并start()动画

    AnimatorSet set = (AnimatorSet) AnimatorInflater.loadAnimator(myContext,
    R.anim.property_animator);
    set.setTarget(myObject);
    set.start();