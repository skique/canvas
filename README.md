# 一、简介
  <canvas> 是 HTML5 新增的元素，并且被大多数浏览器（甚至Internet Explorer 9测试版）支持的2D绘图API）。
  可用于通过使用JavaScript中的脚本来绘制图形。例如，它可以用于绘制图形，制作照片，创建动画，甚至可以进行实时视频处理或渲染。  
  
  Mozilla 程序从 Gecko 1.8 (Firefox 1.5) 开始支持 <canvas>。
  它首先是由 Apple 引入的，用于 OS X Dashboard 和 Safari。
  Chrome、Apple Safari和Opera 9+ 以及Mozilla Firefox等大多数浏览器都支持 <canvas>。
  Internet Explorer 从IE9开始<canvas> ，更旧版本的IE可以引入 Google 的 Explorer Canvas 项目中的脚本来获得<canvas>支持。
  这个补丁的前提是创建使用 JavaScript 代码的元素。例如，可以使用代码段 document.createElement('canvas') 创建一个可识别的 Canvas 标记；但是，这并不意味着有东西经过元素本身 。一个流行的解决方法是包含一个完整的基于 canvas 的 JavaScript 库，这个库由 Google 提供，称为 ExplorerCanvas— 或简称 excanvas。下载并将其作为一个外部文件引用，如下所示。
  `<!--[if IE]>
  `  <script type="text/javascript" src="excanvas.js"></script>
  `<![endif]-->
  

# 二、使用方法
**一个模板骨架**
```<html>
  <head>
    <title>Canvas tutorial</title>
    <script type="text/javascript">
      function draw(){
        var canvas = document.getElementById('tutorial');
        if (canvas.getContext){
          var ctx = canvas.getContext('2d');
        }
      }
    </script>
    <style type="text/css">
      canvas { border: 1px solid black; }
    </style>
  </head>
  <body onload="draw();">
    <canvas id="tutorial" width="150" height="150"></canvas>
  </body>
</html>   
```
## 1、绘制图形
### a、栅格
通常来说网格中的一个单元相当于canvas元素中的一像素。栅格的起点为左上角（坐标为（0,0））。所有元素的位置都相对于原点定位。  
### b、绘制矩形
`fillRect(x, y, width, height)`绘制一个填充的矩形    
`strokeRect(x, y, width, height)`绘制一个矩形的边框   
`clearRect(x, y, width, height)`清除指定矩形区域，让清除部分完全透明。    
`rect(x, y, width, height)`绘制一个左上角坐标为（x,y），宽高为width以及height的矩形 
### c、绘制路径
`beginPath()` 新建一条路径，生成之后，图形绘制命令被指向到路径上生成路径。   
`closePath()` 闭合路径之后图形绘制命令又重新指向到上下文中。   
`stroke()`  通过线条来绘制图形轮廓。    
`fill()` 通过填充路径的内容区域生成实心的图形。    
`clip()` 剪裁路径   
>当你调用fill()函数时，所有没有闭合的形状都会自动闭合，所以你不需要调用closePath()函数。但是调用stroke()时不会自动闭合
#### 移动笔触
`moveTo(x, y)` 将笔触移动到指定的坐标x以及y上。   
#### 线
`lineTo(x, y)`绘制直线，需要用到的方法lineTo()。
#### 圆弧
`arc(x, y, radius, startAngle, endAngle, anticlockwise)`   
画一个以（x,y）为圆心的以radius为半径的圆弧（圆），从startAngle开始到endAngle结束，按照anticlockwise给定的方向（默认为顺时针）来生成。  
>参数anticlockwise 为一个布尔值。为true时，是逆时针方向，否则顺时针方向
注意：arc()函数中的角度单位是弧度，不是度数。角度与弧度的js表达式:radians=(Math.PI/180)*degrees。
#### 二次贝塞尔曲线
`quadraticCurveTo(cp1x, cp1y, x, y)`   
绘制二次贝塞尔曲线，cp1x,cp1y为一个控制点，x,y为结束点。
三次贝塞尔曲线   
`bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)`   
绘制三次贝塞尔曲线，cp1x,cp1y为控制点一，cp2x,cp2y为控制点二，x,y为结束点。  
![](https://mdn.mozillademos.org/files/223/Canvas_curves.png)  
### d、Path2D 对象
#### Path2D()
```   
  new Path2D();     // 空的Path对象
  new Path2D(path); // 克隆Path对象
  new Path2D(d);    // 从SVG建立Path对象
```
####  `Path2D.addPath(path [, transform]) `
添加了一条路径到当前路径（可能添加了一个变换矩阵）
## 2、使用样式和颜色
### a、色彩 Colors
`fillStyle = color`设置图形的填充颜色。   
`strokeStyle = color`设置图形轮廓的颜色。
>颜色值接受rgba()可设置透明度
### b、透明度 Transparency
`globalAlpha = transparencyValue`   
这个属性影响到 canvas 里所有图形的透明度，有效的值范围是 0.0 （完全透明）到 1.0（完全不透明），默认是 1.0。另一种方式是通过css颜色值标准的rgba的形式改变
元素的透明度
### c、线型 Line styles
`lineWidth = value`设置线条宽度。   
`lineCap = type`设置线条末端样式。(可选项：butt，round 和 square。默认是 butt)   
![](https://developer.mozilla.org/@api/deki/files/88/=Canvas_linecap.png)   
`ineJoin = type`设定线条与线条间接合处的样式。(可选项：round, bevel 和 miter。默认是 miter)     
![](https://developer.mozilla.org/@api/deki/files/89/=Canvas_linejoin.png)   
`miterLimit = value`  限制当两条线相交时交接处最大长度；   
![](https://mdn.mozillademos.org/files/240/Canvas_miterlimit.png)   
**使用虚线**   
`getLineDash()` 返回一个包含当前虚线样式，长度为非负偶数的数组。   
`setLineDash(segments)` 设置当前虚线样式。   
`lineDashOffset = value` 设置虚线样式的起始偏移量
### d、渐变
`createLinearGradient(x1, y1, x2, y2)`   
createLinearGradient 方法接受 4 个参数，表示渐变的起点 (x1,y1) 与终点 (x2,y2)   
`createRadialGradient(x1, y1, r1, x2, y2, r2)`   
createRadialGradient 方法接受 6 个参数，前三个定义一个以 (x1,y1) 为原点，半径为 r1 的圆，
后三个参数则定义另一个以 (x2,y2) 为原点，半径为 r2 的圆。
### e、图案样式 Patterns
`createPattern(image, type)`   
该方法接受两个参数。Image 可以是一个 Image 对象的引用，或者另一个 canvas 对象。
Type 必须是下面的字符串值之一：repeat，repeat-x，repeat-y 和 no-repeat。
>用 canvas 对象作为 Image 参数在 Firefox 1.5 (Gecko 1.8) 中是无效的。
### f、阴影 Shadows
`shadowOffsetX = float`   
shadowOffsetX 和 shadowOffsetY 用来设定阴影在 X 和 Y 轴的延伸距离，它们是不受变换矩阵所影响的。负值表示阴影会往上或左延伸，正值则表示会往下或右延伸，它们默认都为 0。   
`shadowOffsetY = float`   
shadowOffsetX 和 shadowOffsetY 用来设定阴影在 X 和 Y 轴的延伸距离，它们是不受变换矩阵所影响的。负值表示阴影会往上或左延伸，正值则表示会往下或右延伸，它们默认都为 0。   
`shadowBlur = float`   
shadowBlur 用于设定阴影的模糊程度，其数值并不跟像素数量挂钩，也不受变换矩阵的影响，默认为 0。   
`shadowColor = color`   
shadowColor 是标准的 CSS 颜色值，用于设定阴影颜色效果，默认是全透明的黑色。
## 3、绘制文本
`fillText(text, x, y [, maxWidth])`   
在指定的(x,y)位置填充指定的文本，绘制的最大宽度是可选的.     
`strokeText(text, x, y [, maxWidth])`   
在指定的(x,y)位置绘制文本边框，绘制的最大宽度是可选的.
## 4、变形 Transformations
### a、保存和恢复
`save()restore()`用来保存和恢复 canvas 状态的，都没有参数
### b、移动 Translating
`translate(x, y)` 它用来移动 canvas 和它的原点到一个不同的位置。   
`translate` 方法接受两个参数。x 是左右偏移量，y 是上下偏移量
### c、旋转 Rotating
### d、缩放 Scaling
 `scale(x, y)`   
 scale 方法接受两个参数。x,y 分别是横轴和纵轴的缩放因子，它们都必须是正值。值比 1.0 小表示缩小，比 1.0 大则表示放大，值为 1.0 时什么效果都没有。
### e、变形 Transforms
`transform(m11, m12, m21, m22, dx, dy)`当前的变形矩阵乘上一个基于自身参数的矩阵   
`setTransform(m11, m12, m21, m22, dx, dy)`将当前的变形矩阵重置为单位矩阵，
>m11：水平方向的缩放
>m12：水平方向的偏移
>m21：竖直方向的偏移
>m22：竖直方向的缩放
>dx：水平方向的移动
>dy：竖直方向的移动
## 5、基本动画
### 动画的基本步骤
* **清空 canvas**        
除非接下来要画的内容会完全充满 canvas （例如背景图），否则你需要清空所有。最简单的做法就是用 clearRect 方法。
* **保存 canvas 状态**          
如果你要改变一些会改变 canvas 状态的设置（样式，变形之类的），又要在每画一帧之时都是原始状态的话，你需要先保存一下。
* **绘制动画图形（animated shapes）**        
这一步才是重绘动画帧。
* **恢复 canvas 状态**       
如果已经保存了 canvas 的状态，可以先恢复它，然后重绘下一帧。
### 操控动画 
**有安排的更新画布**   
`setInterval(function, delay)`当设定好间隔时间后，function会定期执行。   
`setTimeout(function, delay)`在设定好的时间之后执行函数   
`requestAnimationFrame(callback)`告诉浏览器你希望执行一个动画，并在重绘之前，请求浏览器执行一个特定的函数来更新动画。
> window.requestAnimationFrame()可以实现动画效果。这个方法提供了更加平缓并更加有效率的方式来执行动画，当系统准备好了重绘条件的时候，才调用绘制动画帧。一般每秒钟回调函数执行60次，也有可能会被降低。
# 三、资源
### 通用
- [Canvas Web API接口](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API)
- [HTML5 画布](http://wiki.jikexueyuan.com/project/html5/canvas.html)
### 库
- [Fabric.js 具有SVG解析功能的开源canvas库](http://fabricjs.com/)
- [Kinetic.js 专注于桌面与移动应用中的交互操作的开源canvas库](https://github.com/ericdrowell/KineticJS)
- [Paper.js 运行于HTML5 Canvas上的开源矢量图形脚本框架](http://paperjs.org/)
- [Origami.js 开源的轻量的canvas库](http://origamijs.com/docs/)
- [libCanvas 强大轻量的canvas框架](http://libcanvas.github.io/)
- [Processing.js 用于处理可视化语言](http://processingjs.org/)
- [PlayCanvas 一个开源的游戏引擎](https://playcanvas.com/)
- [Pixi.js 一个开源的游戏引擎](http://www.pixijs.com/)
- [PlotKit 图表库](http://www.liquidx.net/plotkit/)
- [Rekapi 关键帧动画库](https://github.com/jeremyckahn/rekapi)
- [PhiloGL 用于数据可视化，创意编程和游戏开发的WebGL框架](http://www.senchalabs.org/philogl/)
- [JavaScript InfoVis Toolkit 创建交互式的2D Canvas数据可视化](http://philogb.github.io/jit/)
- [EaselJS 类Flash API的canvas库](http://www.createjs.com/easeljs)
- [Scrawl-canvas 用来创建2d图形的开源库](http://scrawl.rikweb.org.uk/)
- [heatmap.js 基于 heatmaps 的 canvas 开源库](https://www.patrick-wied.at/static/heatmapjs/)
# 四、canvas创意
>canvas 最重要的是给了你纸和笔，你要考虑用纸和笔能画出什么来，而不是“熟悉用笔戳纸有何用”。      
- [一个基于canvas实现的动效库](https://tonytony.club/spark/  )
