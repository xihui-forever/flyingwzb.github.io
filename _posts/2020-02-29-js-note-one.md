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
---

# Canvas
### 定义

>画布（canvas）元素是HTML5的一部分，允许脚本语言（scripting languages）动态渲染位图像。

*来自维基百科*
### 基本用法
`<canvas id="myCanvas" width="" height=""></canvas>//在内部设置css属性，防止绘制出的图像扭曲`

`var ctx = document.getElementById("").getContext("2d")`

> canvas 元素创造了一个固定大小的画布，它公开了一个或多个渲染上下文，其可以用来绘制和处理要展示的内容（其他种类的上下文也许提供了不同种类的渲染方式，比如， WebGL 使用了基于OpenGL ES的3D上下文 ("experimental-webgl")。）canvas起初是空白的，为了展示，首先脚本需要找到渲染上下文，然后在它的上面绘制。canvas 元素有一个叫做 getContext() 的方法，这个方法是用来获得渲染上下文和它的绘画功能。getContext()只有一个参数，上下文的格式。对于2D图像而言，可以使用 CanvasRenderingContext2D。

### 绘制
#### Path

- beginPath()路径起始
- closePath()路径结束
- fill()填充内容区域生成实心图形
- stroke()通过线条来绘制图形轮廓
- clear()清除
- clip()裁剪

#### Line
##### line style

- linewidth=value
- lineCap=tap//设置线条末端样式:butt，round 和 square,默认是 butt
- lineJoin=type//设定线条与线条间接合处的样式:round, bevel 和 miter,默认是 miter
- miterLimit=value//限制当两条线相交时交接处最大长度；所谓交接处长度（斜接长度）是指线条交接处内角顶点到外角顶点的长度
- getLineDash()//返回一个包含当前虚线样式，长度为非负偶数的数组
- setLineDash(segments)//设置当前虚线样式
- lineDashOffSet=value//设置虚线样式的起始偏移量

`ctx.moveTo(x,y)//start`

`ctx.lineTo(x,y)//end`
##### BeizierCurve

```
ctx.moveTo(x1,y1);
ctx.quadraticCurveTo(cp1x,cp1y,x2,y2);//二次贝塞尔曲线;cp:控制点;（x1,y1):曲线起点;(x2,y2):曲线终点
ctx.bazierCurveTo(cp2x,cp2y,cp3x,cp3y,x1,y1);//三次贝塞尔曲线(两个控制点）
```

#### Circle

`ctx.arc(x,y,r,startAngle,stopAngle,anticlockwise)//startAngle:起始角度；stopAngle:结束角度（均以弧度表示，顺时针方向画圆，圆心平行右端为0度)`

```
ctx.begiPath();
ctx.arc(95,50,40,0,2*Math.PI,true/false);//Math.PI=180度;true:逆时针方向,false:顺时针方向
ctx.stroke();//边框
```

#### Text

1. font = fontfamily
2. textAlign = value(start, end, left, right or center,默认值是 start)
2. textBaseline:
- top of em square
- hanging baseline
- middle
- alphabetic baseline
- bottom of em square
3. shadow

`ctx.shadowOffsetX = float;//X or Y;float:延伸距离`

`ctx.shadowBlur = float;//模糊程度`

`ctx.shadowColor = color;//默认颜色：黑色`

`ctx.fillText(text,x,y)//fillText:填充字体;x,y:栅格坐标`

`ctx.strokeText(text,x,y)`
#### Gradient

- Line渐变：`grd=ctx.createLinearGradient(x,y,x1,y1)`
- 径向/圆渐变：`grd=ctx.createRadialGradient(x,y,r,x1,y1,r1)`

*e.g.渐变星空*
```
var grd=ctx.createLinearGradient(0,0,0,800);// create gradient
	grd.addColorStop(0,'#000080');
	grd.addColorStop(0.1,'#659EC7');
	grd.addColorStop(0.2,'#56A5EC');
	grd.addColorStop(0.3,'#82CAFA');
	grd.addColorStop(0.8,'#fff');
	// fill gradient
    ctx.fillStyle=grd;
	ctx.fillRect(0,0,1200,800);
```
#### Transparency

- globalAlpha = tansparencyValue

#### Image

1. 使用相同页面内的图片
- document.images集合
- document.getElementsByTagName()方法
- 如果你知道你想使用的指定图片的ID，你可以用document.getElementById()获得这个图片
2. 使用其它域名下的图片
- 在 HTMLImageElement上使用crossOrigin属性，你可以请求加载其它域名上的图片(如果图片的服务器允许跨域访问这个图片，那么你可以使用这个图片而不污染canvas，否则，使用这个图片将会污染canvas)
3. 使用其它 canvas 元素
-  document.getElementsByTagName 或 document.getElementById 方法来获取其它 canvas 元素,引入的应该是已经准备好的 canvas
4. 由零开始创建图像
- 用脚本创建一个新的 HTMLImageElement 对象,使用Image()构造函数

```
 var img = new Image();
    img.onload = function(){//用load事件来保证不会在加载完毕之前使用这个图片
      ctx.drawImage(img,x,y);//执行draw语句
    }
    img.src = 'url';
```

#### Translate
`ctx.translate(x,y)//x:左右偏移量;y:上下偏移量`

*e.g.洒星星*
```
// draw stars
for (var j=1;j<100;j++){
	ctx.save();//保存
	// draw stars randomly
	ctx.translate(Math.floor(Math.random()*1200),Math.floor(Math.random()*120));
	drawStar(ctx,Math.floor(Math.random()*3)+2);
	// restorm former canvas
	ctx.restore();//恢复
function drawStar(ctx,r){
	ctx.save();
	ctx.beginPath()
	ctx.moveTo(r,0);
	for (var i=0;i<9;i++){
		ctx.rotate(Math.PI/5);//旋转:rotate(angle);顺时针方向,以弧度为单位,旋转的中心点是canvas的原点,可用translate方法改变原点坐标
		if(i%2 == 0) {
			ctx.lineTo((r/0.525731)*0.200811,0);
		} else {
			ctx.lineTo(r,0);
		}
	}
	ctx.closePath();
	ctx.fill();
	ctx.restore();
}
```

#### Animation
##### Basic Steps

1. 清空 canvas: clearRect 方法
2. 保存 canvas 状态: 在每画一帧之时都是原始状态时做一些改变,先保存一下canvas
3. 绘制动画图形（animated shapes): 重绘动画帧
4. 恢复 canvas 状态: 先恢复它，然后重绘下一帧

##### Scheduled Updates

- setInterval(function, delay)当设定好间隔时间后，function会定期执行
- setTimeout(function, delay)在设定好的时间之后执行函数
- requestAnimationFrame(callback)告诉浏览器你希望执行一个动画，并在重绘之前，请求浏览器执行一个特定的函数来更新动画

*e.g.动画时钟*

```
function clock(){
  var now = new Date();
  var ctx = document.getElementById('canvas').getContext('2d');
  ctx.save();
  ctx.clearRect(0,0,150,150);
  ctx.translate(75,75);
  ctx.scale(0.4,0.4);
  ctx.rotate(-Math.PI/2);
  ctx.strokeStyle = "black";
  ctx.fillStyle = "white";
  ctx.lineWidth = 8;
  ctx.lineCap = "round";

  // Hour marks
  ctx.save();
  for (var i=0;i<12;i++){
    ctx.beginPath();
    ctx.rotate(Math.PI/6);
    ctx.moveTo(100,0);
    ctx.lineTo(120,0);
    ctx.stroke();
  }
  ctx.restore();

  // Minute marks
  ctx.save();
  ctx.lineWidth = 5;
  for (i=0;i<60;i++){
    if (i%5!=0) {
      ctx.beginPath();
      ctx.moveTo(117,0);
      ctx.lineTo(120,0);
      ctx.stroke();
    }
    ctx.rotate(Math.PI/30);
  }
  ctx.restore();
 
  var sec = now.getSeconds();
  var min = now.getMinutes();
  var hr  = now.getHours();
  hr = hr>=12 ? hr-12 : hr;

  ctx.fillStyle = "black";

  // write Hours
  ctx.save();
  ctx.rotate( hr*(Math.PI/6) + (Math.PI/360)*min + (Math.PI/21600)*sec )
  ctx.lineWidth = 14;
  ctx.beginPath();
  ctx.moveTo(-20,0);
  ctx.lineTo(80,0);
  ctx.stroke();
  ctx.restore();

  // write Minutes
  ctx.save();
  ctx.rotate( (Math.PI/30)*min + (Math.PI/1800)*sec )
  ctx.lineWidth = 10;
  ctx.beginPath();
  ctx.moveTo(-28,0);
  ctx.lineTo(112,0);
  ctx.stroke();
  ctx.restore();
 
  // Write seconds
  ctx.save();
  ctx.rotate(sec * Math.PI/30);
  ctx.strokeStyle = "#D40000";
  ctx.fillStyle = "#D40000";
  ctx.lineWidth = 6;
  ctx.beginPath();
  ctx.moveTo(-30,0);
  ctx.lineTo(83,0);
  ctx.stroke();
  ctx.beginPath();
  ctx.arc(0,0,10,0,Math.PI*2,true);
  ctx.fill();
  ctx.beginPath();
  ctx.arc(95,0,10,0,Math.PI*2,true);
  ctx.stroke();
  ctx.fillStyle = "rgba(0,0,0,0)";
  ctx.arc(0,0,3,0,Math.PI*2,true);
  ctx.fill();
  ctx.restore();

  ctx.beginPath();
  ctx.lineWidth = 14;
  ctx.strokeStyle = '#325FA2';
  ctx.arc(0,0,142,0,Math.PI*2,true);
  ctx.stroke();

  ctx.restore();

  window.requestAnimationFrame(clock);
}

window.requestAnimationFrame(clock);
```

#### Tips
- 不可见的Canvas复制到页面可见的Canvas
- 尽量使用整数坐标（ps:注意lineWidth加了closePath()的半渲染状态）
- 建多个重叠的Canvas，绘制不同的层
- 若背景图片不变，则直接用img元素放在最底层
