---
layout:     post
title:      js初步笔记
subtitle:   canvas/jQuery/ajax
date:       2020-02-29
author:     X. yuan
header-img: img/post-bg-article.jpg
catalog: true
tags:
    - IDEA
    - js


##canvas
###定义
>画布（canvas）元素是HTML5的一部分，允许脚本语言（scripting languages）动态渲染位图像。
*来自维基百科*
###基本用法
`<canvas id="myCanvas" width="" height=""></canvas>`
`var ctx = document.getElementById("").getContext("2d")`
><canvas> 元素创造了一个固定大小的画布，它公开了一个或多个渲染上下文，其可以用来绘制和处理要展示的内容。我们将会将注意力放在2D渲染上下文中。其他种类的上下文也许提供了不同种类的渲染方式；比如， WebGL 使用了基于OpenGL ES的3D上下文 ("experimental-webgl") 。
>canvas起初是空白的。为了展示，首先脚本需要找到渲染上下文，然后在它的上面绘制。<canvas> 元素有一个叫做 getContext() 的方法，这个方法是用来获得渲染上下文和它的绘画功能。getContext()只有一个参数，上下文的格式。对于2D图像而言，如本教程，你可以使用 CanvasRenderingContext2D。
###绘制
####line
`ctx.moveTo(x,y)//start`
`ctx.lineTo(x,y)//end`
####circle
`ctx.arc(x,y,r,start,stop)//start:起始角度；stop:结束角度（均以弧度表示，顺时针方向画圆，圆心平行右端为0度`

