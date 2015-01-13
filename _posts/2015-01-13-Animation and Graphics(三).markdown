---
layout: post
title:  "Animation and Graphics(三)"
date:   2015-01-13 16:52:30
categories: jekyll update
---

## **视图动画** ##
存在位置：
	res/anim

对于设置相对位置："50" for 50% relative to the parent, or "50%" for 50% relative to itself

如何使用：
    
    ImageView spaceshipImage = (ImageView) findViewById(R.id.spaceshipImage);
    Animation hyperspaceJumpAnimation = AnimationUtils.loadAnimation(this, R.anim.hyperspace_jump);
    spaceshipImage.startAnimation(hyperspaceJumpAnimation);

## **Drawable动画** ##
AnimationDrawable是Drawable动画的基类
存在于res/drawable文件夹

    <animation-list xmlns:android="http://schemas.android.com/apk/res/android"
    android:oneshot="true">
    <item android:drawable="@drawable/rocket_thrust1" android:duration="200" />
    <item android:drawable="@drawable/rocket_thrust2" android:duration="200" />
    <item android:drawable="@drawable/rocket_thrust3" android:duration="200" />
    </animation-list>
