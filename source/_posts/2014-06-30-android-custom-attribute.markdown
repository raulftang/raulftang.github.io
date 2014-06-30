---
layout: post
title: "Android custom attribute"
date: 2014-06-30 16:11:07 +0800
comments: true
author: Raulf
categories: [android, attribute]
---
Android中自定义属性的使用

做Android布局是件很享受的事，这得益于他良好的xml方式。使用xml可以快速有效的为软件定义界面。可是有时候我们总感觉官方定义的一些基本组件不够用，自定义组件就不可避免了。那么如何才能做到像官方提供的那些组件一样用xml来定义他的属性呢？现在我们就来讨论一下他的用法。

一、在res/values文件下定义一个attrs.xml文件，代码如下：

<?xml version="1.0" encoding="utf-8"?> 
<resources> 
    <declare-styleable name="ToolBar"> 
        <attr name="buttonNum" format="integer"/> 
        <attr name="itemBackground" format="reference|color"/> 
    </declare-styleable> 
</resources>

二、在布局xml中如下使用该属性:

<?xml version="1.0" encoding="utf-8"?> 
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android" 
    xmlns:toolbar="http://schemas.android.com/apk/res/cn.zzm.toolbar" 
    android:orientation="vertical" 
    android:layout_width="fill_parent" 
    android:layout_height="fill_parent" 
    > 
    <cn.zzm.toolbar.ToolBar android:id="@+id/gridview_toolbar" 
        android:layout_width="fill_parent" 
        android:layout_height="wrap_content" 
        android:layout_alignParentBottom="true" 
        android:background="@drawable/control_bar" 
        android:gravity="center" 
        toolbar:buttonNum="5" 
        toolbar:itemBackground="@drawable/control_bar_item_bg"/> 
</RelativeLayout>

三、在自定义组件中，可以如下获得xml中定义的值：

TypedArray a = context.obtainStyledAttributes(attrs,R.styleable.ToolBar); 
buttonNum = a.getInt(R.styleable.ToolBar_buttonNum, 5); 
itemBg = a.getResourceId(R.styleable.ToolBar_itemBackground, -1);

a.recycle();

就这么简单的三步，即可完成对自定义属性的使用。

*********************************************************************

好了，基本用法已经讲完了，现在来看看一些注意点和知识点吧。

首先来看看attrs.xml文件。

该文件是定义属性名和格式的地方，需要用<declare-styleable name="ToolBar"></declare-styleable>包围所有属性。其中name为该属性集的名字，主要用途是标识该属性集。那在什么地方会用到呢？主要是在第三步。看到没？在获取某属性标识时，用到"R.styleable.ToolBar_buttonNum"，很显然，他在每个属性前面都加了"ToolBar_"。

在来看看各种属性都有些什么类型吧：string , integer , dimension , reference , color , enum.

前面几种的声明方式都是一致的，例如：<attr name="buttonNum" format="integer"/>。 
只有enum是不同的，用法举例：

<attr name="testEnum"> 
    <enum name="fill_parent" value="-1"/> 
    <enum name="wrap_content" value="-2"/> 
</attr>

如果该属性可同时传两种不同的属性，则可以用“|”分割开即可。

让我们再来看看布局xml中需要注意的事项。

首先得声明一下：xmlns:toolbar=http://schemas.android.com/apk/res/cn.zzm.toolbar 
注意，“toolbar”可以换成其他的任何名字，后面的url地址必须最后一部分必须用上自定义组件的包名。自定义属性了，在属性名前加上“toolbar”即可。

 

最后来看看java代码中的注意事项。

在自定义组件的构造函数中，用

TypedArray a = context.obtainStyledAttributes(attrs,R.styleable.ToolBar);

来获得对属性集的引用，然后就可以用“a”的各种方法来获取相应的属性值了。这里需要注意的是，如果使用的方法和获取值的类型不对的话，则会返回默认值。因此，如果一个属性是带两个及以上不用类型的属性，需要做多次判断，知道读取完毕后才能判断应该赋予何值。当然，在取完值的时候别忘了回收资源哦！
