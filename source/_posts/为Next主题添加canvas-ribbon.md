---
title: 为你的网站添加动画彩带背景-尤雨溪博客
comment: true
date: 2017-04-10 20:57:25
tags: [Hexo, GitHub, Canvas,Next,JavaScript]
cover: http://ortur5wom.bkt.clouddn.com/cover-ribbon.jpg
---

一直关注着前端框架Vue的作者尤雨溪，前些天浏览到他的博客，被他博客的动画背景深深吸引。大体效果如下图。

![ScreenClip](http://ortur5wom.bkt.clouddn.com/canvas1.png)

是的，一条飘逸、灵动的彩带！但更Amazing的是当用用户点击页面时，背景彩带也会实时响应点击事件，随机生成不同路径、不同颜色的新彩带。我瞬间中了这变幻莫测彩带条的毒，呆呆地点了半天的屏幕，好玩到根本停不下来。。


接下来，让我们看一下如何实现这一canvas背景彩带的效果。其实只需要简单的几十行代码，就能轻松实现。然后就可以添加到自己的个人网站了。

# 用到的知识

js基础+canvas

# 实现思路

## 1.canvas的使用

`CanvasRenderingContext2D` 接口提供的 2D 渲染背景用来绘制[canvas](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/canvas)元素，为了获得这个接口的对象，需要在`<canvas>`上调用 [getContext()](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLCanvasElement/getContext) ，并提供一个 `"2d"` 的参数：

```javascript
var canvas = document.getElementById('tutorial');
var ctx = canvas.getContext('2d');
```

一旦你得到 2D 渲染背景后，你可以像下面一样绘制：

（以绘制`路径`为例，此外矩形、文本、图像等）

```javascript
ctx.beginPath();
ctx.strokeStyle = 'blue';
ctx.moveTo(10,50);
ctx.lineTo(200,50);
ctx.stroke();
```

绘制效果如下：

![ScreenClip1](http://ortur5wom.bkt.clouddn.com/canvas2.png)

## 2.绘制折线

对以上代码稍作改变，使直线变为折线：

``` javascript
ctx.beginPath();
ctx.strokeStyle = 'blue';
ctx.moveTo(10,50);
ctx.lineTo(100,80);
ctx.lineTo(200,20);
ctx.lineTo(300,40);
ctx.stroke();
ctx.closePath();
```

绘制效果如下：

![ScreenClip2](http://ortur5wom.bkt.clouddn.com/canvas3.png)

此时，一条与最终要实现的背景彩带在大体形状上已初具雏形。只是缺少自动绘制、路径宽度和其中每一段路径上随机的颜色。

## 3.绘制背景彩带

以下是我的源代码，其中已有详细的注释信息，相信你一定能够看懂的。复制一下代码，保存为html文件，浏览器打开即可运行查看。

``` html
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
<canvas></canvas>
<script>
    document.addEventListener('touchmove', function (e) {
        e.preventDefault()
    });
    var canvasRibbon = document.getElementsByTagName('canvas')[0],
        ctx = canvasRibbon.getContext('2d'),    // 获取canvas 2d上下文
        dpr = window.devicePixelRatio || 1, // the size of one CSS pixel to the size of one physical pixel.
        width = window.innerWidth,     // 返回窗口的文档显示区的宽高
        height = window.innerHeight,
        RIBBON_WIDE = 90,
        path,
        math = Math,
        r = 0,
        PI_2 = math.PI * 2,    // 圆周率*2
        cos = math.cos,   // cos函数返回一个数值的余弦值（-1~1）
        random = math.random;   // 返回0-1随机数

    canvasRibbon.width = width * dpr;     // 返回实际宽高
    canvasRibbon.height = height * dpr;
    ctx.scale(dpr, dpr);    // 水平、竖直方向缩放
    ctx.globalAlpha = 0.6;  // 图形透明度

    function init() {
        ctx.clearRect(0, 0, width, height);     // 擦除之前绘制内容
        path = [{x: 0, y: height * 0.7 + RIBBON_WIDE}, {x: 0, y: height * 0.7 - RIBBON_WIDE}];
        // 路径没有填满屏幕宽度时，绘制路径
        while (path[1].x < width + RIBBON_WIDE) {
            draw(path[0], path[1])     // 调用绘制方法
        }
    }

    // 绘制彩带每一段路径
    function draw(start, end) {
        ctx.beginPath();    // 创建一个新的路径
        ctx.moveTo(start.x, start.y);   // path起点
        ctx.lineTo(end.x, end.y);   // path终点
        var nextX = end.x + (random() * 2 - 0.25) * RIBBON_WIDE,
            nextY = geneY(end.y);
        ctx.lineTo(nextX, nextY);
        ctx.closePath();

        r -= PI_2 / -50;
        // 随机生成并设置canvas路径16进制颜色
        ctx.fillStyle = '#' + (cos(r) * 127 + 128 << 16 | cos(r + PI_2 / 3) * 127 + 128 << 8 | cos(r + PI_2 / 3 * 2) * 127 + 128).toString(16);
        ctx.fill();     // 根据当前样式填充路径
        path[0] = path[1];    // 起点更新为当前终点
        path[1] = {x: nextX, y: nextY}     // 更新终点
    }

    // 获取下一路径终点的y坐标值
    function geneY(y) {
        var temp = y + (random() * 2 - 1.1) * RIBBON_WIDE;
        return (temp > height || temp < 0) ? geneY(y) : temp
    }

    document.onclick = init;
    document.ontouchstart = init;
    init();
</script>
</body>
</html>
```

此外我还提供[简单一条命令为你的网页添加canvas-ribbon背景彩带](https://github.com/zproo/canvas-ribbon)(Github源码)的方法。你可以直接访问我的github仓库获得，只需要一行代码就可以为你的网站添加上面介绍的canvas-ribbon背景彩带了。

# 后记

在为我自己的博客添加成功后，我还对Hexo 的Next主题添加了这一功能（只适用于Next的Pisces主题），并给Next作者发了PR。

现在此提交已被作者Merge，所以如果你的博客使用了Hexo的Next主题，只需要在`主题配置文件`中找到`canvas-ribbon`属性，并设置为`true`，即可拥有背景彩带的效果。

![ScreenClip3](http://ortur5wom.bkt.clouddn.com/canvas4.png)

# 参考资料

- [CanvasRenderingContext2D-路径](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/beginPath)
- [evanyou](http://evanyou.me/)