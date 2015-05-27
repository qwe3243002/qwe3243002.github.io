---
layout: default
title:  "Animation and Graphics(一)"
date:   2015-01-12 16:09:30
categories: Android
tags: 动画
---
## 动画 ##

Android框架提供两种动画体系属性动画（从Android3.0引入），视图动画。虽然两种动画系统都能够正常使用，但是建议使用属性动画系统，因为属性动画更加灵活并且能提供更多的特性。除了这两种动画系统外，还可以使用Drawable动画，他能加载Drawable资源并且一帧一帧显示出来。

    1. **属性动画（Property Animation）**
    
    	从Android3.0开始引入，用来动态改变任何对象的属性，甚至是没有呈现在屏幕上的对象，并且你可以用于动态改变自定义类型的属性。
    2. **视图动画（View Animation）**
    	
    	视图动画相对更加简单，并且能满足大多数应用的需求。
    3. **Drawable动画**
    
    	Drawable动画会一帧一帧的显示Drawable资源，在需要不断重复呈现Drawable时，是非常好的选择。

## 2D/3D图形 ##

1.**Canvas与Drawable**

	Android提供了一组View组件，来满足用户通常使用。你也可以继承这些组件，自定义样式与行为来满足自己需求。此外，你可以使用Canvas提供的各种绘图方法来决定你想呈现的图案，或者使用drawable资源。
	
2.**硬件加速**
	
	从Android3.0开始对大部分Canvas绘图API进行硬件加速，以提高性能。
3.**OpenGL**
	
	Android supports OpenGL ES 1.0 and 2.0, with Android framework APIs as well as natively with the Native Development Kit (NDK). Using the framework APIs is desireable when you want to add a few graphical enhancements to your application that are not supported with the Canvas APIs, or if you desire platform independence and don't demand high performance. There is a performance hit in using the framework APIs compared to the NDK, so for many graphic intensive applications such as games, using the NDK is beneficial (It is important to note though that you can still get adequate performance using the framework APIs. For example, the Google Body app is developed entirely using the framework APIs). OpenGL with the NDK is also useful if you have a lot of native code that you want to port over to Android. For more information about using the NDK, read the docs in the docs/ directory of the NDK download.