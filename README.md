# ts-canvas
ts写的canvas教程

## 开始之前

使用 `<canvas>` 元素不是非常难，但你需要一些基本的HTML和JavaScript知识。一些过时的浏览器不支持`<canvas>` 元素，但是所有的新版本主流浏览器都支持它。Canvas 的默认大小为300像素×150像素（宽×高，像素的单位是px）。但是，可以使用HTML的高度和宽度属性来自定义Canvas 的尺寸。为了在 Canvas 上绘制图形，我们使用一个JavaScript上下文对象，它能动态创建图像（ creates graphics on the fly）。

## 基本用法

### canvas基本元素

`<canvas id="tutorial" width="150" height="150"></canvas>`

`<canvas>` 看起来和 `<img>` 元素很相像，唯一的不同就是它并没有 src 和 alt 属性。实际上，`<canvas>` 标签只有两个属性—— width和height。这些都是可选的，并且同样利用 DOM properties 来设置。当没有设置宽度和高度的时候，canvas会初始化宽度为300像素和高度为150像素。该元素可以使用CSS来定义大小，但在绘制时图像会伸缩以适应它的框架尺寸：如果CSS的尺寸与初始画布的比例不一致，它会出现扭曲。

如果你绘制出来的图像是扭曲的, 尝试用width和height属性为`<canvas>`明确规定宽高，而不是使用CSS。

id属性并不是`<canvas>`元素所特有的，而是每一个HTML元素都默认具有的属性（比如class属性）。给每个标签都加上一个id属性是个好主意，因为这样你就能在我们的脚本中很容易的找到它。

`<canvas>`元素可以像任何一个普通的图像一样（有margin，border，background等等属性）被设计。然而，这些样式不会影响在canvas中的实际图像。我们将会在一个专门的章节里看到这是如何解决的。当开始时没有为canvas规定样式规则，其将会完全透明。

### 替换内容

`<canvas>`元素与`<img>`标签的不同之处在于，就像`<video>`，`<audio>`，或者 `<picture>`元素一样，很容易定义一些替代内容。由于某些较老的浏览器（尤其是IE9之前的IE浏览器）或者文本浏览器不支持HTML元素"canvas"，在这些浏览器上你应该总是能展示替代内容。

这非常简单：我们只是在`<canvas>`标签中提供了替换内容。不支持`<canvas>`的浏览器将会忽略容器并在其中渲染后备内容。而支持`<canvas>`的浏览器将会忽略在容器中包含的内容，并且只是正常渲染canvas。

举个例子，我们可以提供对canvas内容的文字描述或者是提供动态生成内容相对应的静态图片，如下所示：

```
<canvas id="stockGraph" width="150" height="150">
  current stock price: $3.15 +0.15
</canvas>

<canvas id="clock" width="150" height="150">
  <img src="https://infinitypro-img.infinitynewtab.com/custom-icon/9001c91g3utjee9xucs5xfhgi0goag.png?imageMogr2/thumbnail/240x/format/webp/blur/1x0/quality/100|imageslim" width="150" height="150" alt=""/>
</canvas>
```

###  </canvas> 标签不可省

与 `<img>` 元素不同，`<canvas>` 元素需要结束标签(`</canvas>`)。如果结束标签不存在，则文档的其余部分会被认为是替代内容，将不会显示出来。

### 渲染上下文（The rendering context）

`<canvas>` 元素创造了一个固定大小的画布，它公开了一个或多个渲染上下文，其可以用来绘制和处理要展示的内容。我们将会将注意力放在2D渲染上下文中。其他种类的上下文也许提供了不同种类的渲染方式；比如， WebGL 使用了基于OpenGL ES的3D上下文 ("experimental-webgl") 。

canvas起初是空白的。为了展示，首先脚本需要找到渲染上下文，然后在它的上面绘制。`<canvas>` 元素有一个叫做 getContext() 的方法，这个方法是用来获得渲染上下文和它的绘画功能。getContext()只有一个参数，上下文的格式。对于2D图像而言，如本教程，你可以使用 CanvasRenderingContext2D。

```
var canvas = document.getElementById('tutorial');
var ctx = canvas.getContext('2d');
```

代码的第一行通过使用 `document.getElementById()` 方法来为 `<canvas>` 元素得到DOM对象。一旦有了元素对象，你可以通过使用它的getContext() 方法来访问绘画上下文。

### 检查支持性

替换内容是用于在不支持 `<canvas>` 标签的浏览器中展示的。通过简单的测试getContext()方法的存在，脚本可以检查编程支持性。上面的代码片段现在变成了这个样子：

```
var canvas = document.getElementById('tutorial');

if (canvas.getContext){
  var ctx = canvas.getContext('2d');
  // drawing code here
} else {
  // canvas-unsupported code here
}
```

### example

这里的是一个最简单的模板，我们之后就可以把它作为之后的例子的起点。

html
```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        canvas { border: 1px solid black; }
    </style>
</head>

<body>
    <canvas id="tutorial" width="150" height="150"></canvas>
    <script src="./dist/index.js"></script>
</body>

</html>
```
ts
```
let canvas = <HTMLCanvasElement> document.getElementById('tutorial')

if (canvas.getContext) {
    let ctx = <CanvasRenderingContext2D> canvas.getContext('2d')
    console.log(2)
}
```

绘制两个长方形

```
let canvas = <HTMLCanvasElement> document.getElementById('tutorial')

if (canvas.getContext) {
    let ctx = <CanvasRenderingContext2D> canvas.getContext('2d')
    ctx.fillStyle = 'rgb(200, 0, 0)'
    ctx.fillRect(10, 10, 55, 50)
    ctx.fillStyle = 'rgba(0, 0, 200, 0.5)'
    ctx.fillRect(30, 30, 55, 50)
}
```

既然我们已经设置了 canvas 环境，我们可以深入了解如何在 canvas 上绘制。到本文的最后，你将学会如何绘制矩形，三角形，直线，圆弧和曲线，变得熟悉这些基本的形状。绘制物体到Canvas前，需掌握路径，我们看看到底怎么做。

## 绘制形状

既然我们已经设置了 canvas 环境，我们可以深入了解如何在 canvas 上绘制。到本文的最后，你将学会如何绘制矩形，三角形，直线，圆弧和曲线，变得熟悉这些基本的形状。绘制物体到Canvas前，需掌握路径，我们看看到底怎么做。

### 栅格

在我们开始画图之前，我们需要了解一下画布栅格（canvas grid）以及坐标空间。上一页中的HTML模板中有个宽150px, 高150px的canvas元素。如右图所示，canvas元素默认被网格所覆盖。通常来说网格中的一个单元相当于canvas元素中的一像素。栅格的起点为左上角（坐标为（0,0））。所有元素的位置都相对于原点定位。所以图中蓝色方形左上角的坐标为距离左边（X轴）x像素，距离上边（Y轴）y像素（坐标为（x,y））。在课程的最后我们会平移原点到不同的坐标上，旋转网格以及缩放。现在我们还是使用原来的设置。

![栅格](https://mdn.mozillademos.org/files/224/Canvas_default_grid.png)

### 绘制矩形

不同于SVG，HTML中的元素canvas只支持一种原生的图形绘制：矩形。所有其他的图形的绘制都至少需要生成一条路径。不过，我们拥有众多路径生成的方法让复杂图形的绘制成为了可能。

首先，我们回到矩形的绘制中。canvas提供了三种方法绘制矩形：

1. 绘制一个填充的矩形

    `ctx.fillRect(x, y, width, height)`

2. 绘制一个矩形的边框

    `ctx.strokeRect(x, y, width, height)`

3. 清除指定矩形区域，让清除部分完全透明。

    `clearRect(x, y, width, height)`

上面提供的方法之中每一个都包含了相同的参数。x与y指定了在canvas画布上所绘制的矩形的左上角（相对于原点）的坐标。width和height设置矩形的尺寸。

* example

    ```
    let canvas = <HTMLCanvasElement> document.getElementById('tutorial')

    if (canvas.getContext) {
        let ctx = <CanvasRenderingContext2D> canvas.getContext('2d')
        ctx.fillRect(25, 25, 100, 100);
        ctx.clearRect(45, 45, 60, 60);
        ctx.strokeRect(50, 50, 50, 50);
    }
    ```

    fillRect()函数绘制了一个边长为100px的黑色正方形。clearRect()函数从正方形的中心开始擦除了一个60`*60px的正方形，接着strokeRect()在清除区域内生成一个50*`50的正方形边框。

    接下来我们能够看到clearRect()的两个可选方法，然后我们会知道如何改变渲染图形的填充颜色及描边颜色。

    不同于下一节所要介绍的路径函数（path function），以上的三个函数绘制之后会马上显现在canvas上，即时生效。

### 绘制路径

    图形的基本元素是路径。路径是通过不同颜色和宽度的线段或曲线相连形成的不同形状的点的集合。一个路径，甚至一个子路径，都是闭合的。使用路径绘制图形需要一些额外的步骤。

1. 首先，你需要创建路径起始点。

2. 然后你使用画图命令去画出路径。

3. 之后你把路径封闭。

4. 一旦路径生成，你就能通过描边或填充路径区域来渲染图形。

以下是所要用到的函数：

1. 新建一条路径，生成之后，图形绘制命令被指向到路径上生成路径。

`ctx.beginPath()`

2. 闭合路径之后图形绘制命令又重新指向到上下文中。

`ctx.closePath()`

3. 通过线条来绘制图形轮廓。

`ctx.stroke()`

4. 通过填充路径的内容区域生成实心的图形。

`ctx.fill()`

生成路径的第一步叫做beginPath()。本质上，路径是由很多子路径构成，这些子路径都是在一个列表中，所有的子路径（线、弧形、等等）构成图形。而每次这个方法调用之后，列表清空重置，然后我们就可以重新绘制新的图形。

**注意**：当前路径为空，即调用beginPath()之后，或者canvas刚建的时候，第一条路径构造命令通常被视为是moveTo（），无论实际上是什么。出于这个原因，你几乎总是要在设置路径之后专门指定你的起始位置。

第二步就是调用函数指定绘制路径，本文稍后我们就能看到了。

第三，就是闭合路径closePath(),不是必需的。这个方法会通过绘制一条从当前点到开始点的直线来闭合图形。如果图形是已经闭合了的，即当前点为开始点，该函数什么也不做。

注意：当你调用fill()函数时，所有没有闭合的形状都会自动闭合，所以你不需要调用closePath()函数。但是调用stroke()时不会自动闭合。

#### example 绘制三角形

```
let canvas = <HTMLCanvasElement> document.getElementById('tutorial')

if (canvas.getContext) {
    let ctx = <CanvasRenderingContext2D> canvas.getContext('2d')
    ctx.beginPath()
    ctx.moveTo(0, 0);
    ctx.lineTo(100, 0);
    ctx.lineTo(100, 100);
    ctx.fill();
}
```

### 移动笔触

一个非常有用的函数，而这个函数实际上并不能画出任何东西，也是上面所描述的路径列表的一部分，这个函数就是moveTo()。或者你可以想象一下在纸上作业，一支钢笔或者铅笔的笔尖从一个点到另一个点的移动过程。

将笔触移动到指定的坐标x以及y上。

`moveTo(x, y)`

当canvas初始化或者beginPath()调用后，你通常会使用moveTo()函数设置起点。我们也能够使用moveTo()绘制一些不连续的路径。看一下下面的笑脸例子。我将用到moveTo()方法（红线处）的地方标记了。

```
let canvas = <HTMLCanvasElement> document.getElementById('tutorial')

if (canvas.getContext) {
    let ctx = <CanvasRenderingContext2D> canvas.getContext('2d')
    ctx.beginPath();
    ctx.arc(75, 75, 50, 0, Math.PI * 2, true); // 绘制
    ctx.moveTo(110, 75);
    ctx.arc(75, 75, 35, 0, Math.PI, false);   // 口(顺时针)
    ctx.moveTo(65, 65);
    ctx.arc(60, 65, 5, 0, Math.PI * 2, true);  // 左眼
    ctx.moveTo(95, 65);
    ctx.arc(90, 65, 5, 0, Math.PI * 2, true);  // 右眼
    ctx.stroke();
}
```

如果你想看到连续的线，你可以移除调用的moveTo()。

### 线

绘制直线，需要用到的方法lineTo()。

绘制一条从当前位置到指定x以及y位置的直线。

`lineTo(x, y)`

该方法有两个参数：x以及y ，代表坐标系中直线结束的点。开始点和之前的绘制路径有关，之前路径的结束点就是接下来的开始点，等等。。。开始点也可以通过moveTo()函数改变。

下面的例子绘制两个三角形，一个是填充的，另一个是描边的。

```
let canvas = <HTMLCanvasElement> document.getElementById('tutorial')

if (canvas.getContext) {
    let ctx = <CanvasRenderingContext2D> canvas.getContext('2d')
    ctx.beginPath();
    // 填充三角形填充
    ctx.beginPath();
    ctx.moveTo(25, 25);
    ctx.lineTo(105, 25);
    ctx.lineTo(25, 105);
    ctx.fill();

    // 描边三角形
    ctx.beginPath();
    ctx.moveTo(125, 125);
    ctx.lineTo(125, 45);
    ctx.lineTo(45, 125);
    ctx.closePath();
    ctx.stroke();
}
```

这里从调用beginPath()函数准备绘制一个新的形状路径开始。然后使用moveTo()函数移动到目标位置上。然后下面，两条线段绘制后构成三角形的两条边。

你会注意到填充与描边三角形步骤有所不同。正如上面所提到的，因为路径使用填充（fill）时，路径自动闭合，使用描边（stroke）则不会闭合路径。如果没有添加闭合路径closePath()到描述三角形函数中，则只绘制了两条线段，并不是一个完整的三角形。

### 圆弧

绘制圆弧或者圆，我们使用arc()方法。当然可以使用arcTo()，不过这个的实现并不是那么的可靠，所以我们这里不作介绍。

画一个以（x,y）为圆心的以radius为半径的圆弧（圆），从startAngle开始到endAngle结束，按照anticlockwise给定的方向（默认为顺时针）来生成。

`arc(x, y, radius, startAngle, endAngle, anticlockwise)`

根据给定的控制点和半径画一段圆弧，再以直线连接两个控制点。

`arcTo(x1, y1, x2, y2, radius)`

这里详细介绍一下arc方法，该方法有六个参数：x,y为绘制圆弧所在圆上的圆心坐标。radius为半径。startAngle以及endAngle参数用弧度定义了开始以及结束的弧度。这些都是以x轴为基准。参数anticlockwise为一个布尔值。为true时，是逆时针方向，否则顺时针方向。

```
注意：arc()函数中表示角的单位是弧度，不是角度。角度与弧度的js表达式:

弧度=(Math.PI/180)*角度。
```

下面的例子比上面的要复杂一下，下面绘制了12个不同的角度以及填充的圆弧。

下面两个for循环，生成圆弧的行列（x,y）坐标。每一段圆弧的开始都调用beginPath()。代码中，每个圆弧的参数都是可变的，实际编程中，我们并不需要这样做。

x,y坐标是可变的。半径（radius）和开始角度（startAngle）都是固定的。结束角度（endAngle）在第一列开始时是180度（半圆）然后每列增加90度。最后一列形成一个完整的圆。

clockwise语句作用于第一、三行是顺时针的圆弧，anticlockwise作用于二、四行为逆时针圆弧。if语句让一、二行描边圆弧，下面两行填充路径。

这个示例所需的画布大小略大于本页面的其他例子: 150 x 200 像素。

```
let canvas = <HTMLCanvasElement> document.getElementById('tutorial')

if (canvas.getContext) {
    let ctx = <CanvasRenderingContext2D> canvas.getContext('2d')
    for (let i = 0; i < 4; i++) {
        for (let j = 0; j < 3; j++) {
            ctx.beginPath();
            let x = 25 + j * 50; // x 坐标值
            let y = 25 + i * 50; // y 坐标值
            let radius = 20; // 圆弧半径
            let startAngle = 0; // 开始点
            let endAngle = Math.PI + (Math.PI * j) / 2; // 结束点
            let anticlockwise = i % 2 === 0 ? false : true; // 顺时针或逆时针

            ctx.arc(x, y, radius, startAngle, endAngle, anticlockwise);

            if (i > 1) {
                ctx.fill();
            } else {
                ctx.stroke();
            }
        }
    }
}
```

### 二次贝塞尔曲线及三次贝塞尔曲线

下一个十分有用的路径类型就是贝塞尔曲线。二次及三次贝塞尔曲线都十分有用，一般用来绘制复杂有规律的图形。

绘制二次贝塞尔曲线，cp1x,cp1y为一个控制点，x,y为结束点。

`quadraticCurveTo(cp1x, cp1y, x, y)`

绘制三次贝塞尔曲线，cp1x,cp1y为控制点一，cp2x,cp2y为控制点二，x,y为结束点。

`bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)`

图能够很好的描述两者的关系，二次贝塞尔曲线有一个开始点（蓝色）、一个结束点（蓝色）以及一个控制点（红色），而三次贝塞尔曲线有两个控制点。

![贝塞尔](https://mdn.mozillademos.org/files/223/Canvas_curves.png)

参数x、y在这两个方法中都是结束点坐标。cp1x,cp1y为坐标中的第一个控制点，cp2x,cp2y为坐标中的第二个控制点。

使用二次以及三次贝塞尔曲线是有一定的难度的，因为不同于像Adobe Illustrators这样的矢量软件，我们所绘制的曲线没有给我们提供直接的视觉反馈。这让绘制复杂的图形变得十分困难。在下面的例子中，我们会绘制一些简单有规律的图形，如果你有时间以及更多的耐心,很多复杂的图形你也可以绘制出来。

下面的这些例子没有多少困难。这两个例子中我们会连续绘制贝塞尔曲线，最后形成复杂的图形。

#### 二次贝塞尔曲线

这个例子使用多个贝塞尔曲线来渲染对话气泡。

```
let canvas = <HTMLCanvasElement> document.getElementById('tutorial')

if (canvas.getContext) {
    let ctx = <CanvasRenderingContext2D> canvas.getContext('2d')
    ctx.beginPath();
    ctx.moveTo(75, 25);
    ctx.quadraticCurveTo(25, 25, 25, 62.5);
    ctx.quadraticCurveTo(25, 100, 50, 100);
    ctx.quadraticCurveTo(50, 120, 30, 125);
    ctx.quadraticCurveTo(60, 120, 65, 100);
    ctx.quadraticCurveTo(125, 100, 125, 62.5);
    ctx.quadraticCurveTo(125, 25, 75, 25);
    ctx.stroke();
}
```

#### 三次贝塞尔曲线

这个例子使用贝塞尔曲线绘制心形。

```
let canvas = <HTMLCanvasElement> document.getElementById('tutorial')

if (canvas.getContext) {
    let ctx = <CanvasRenderingContext2D> canvas.getContext('2d')
    ctx.beginPath();
    ctx.moveTo(75, 40);
    ctx.bezierCurveTo(75, 37, 70, 25, 50, 25);
    ctx.bezierCurveTo(20, 25, 20, 62.5, 20, 62.5);
    ctx.bezierCurveTo(20, 80, 40, 102, 75, 120);
    ctx.bezierCurveTo(110, 102, 130, 80, 130, 62.5);
    ctx.bezierCurveTo(130, 62.5, 130, 25, 100, 25);
    ctx.bezierCurveTo(85, 25, 75, 37, 75, 40);
    ctx.fill();
}
```

### 矩形

直接在画布上绘制矩形的三个额外方法，正如我们开始所见的绘制矩形，同样，也有rect()方法，将一个矩形路径增加到当前路径上。

绘制一个左上角坐标为（x,y），宽高为width以及height的矩形。

`rect(x, y, width, height)`

当该方法执行的时候，moveTo()方法自动设置坐标参数（0,0）。也就是说，当前笔触自动重置回默认坐标。

### 组合使用

目前为止，每一个例子中的每个图形都只用到一种类型的路径。然而，绘制一个图形并没有限制使用数量以及类型。所以在最后的一个例子里，让我们组合使用所有的路径函数来重现一款著名的游戏。

```
let canvas = <HTMLCanvasElement> document.getElementById('tutorial')

// tslint:disable-next-line:max-line-length
function roundedRect(ctx: CanvasRenderingContext2D, x: number, y: number, width: number, height: number, radius: number) {
    ctx.beginPath();
    ctx.moveTo(x, y + radius);
    ctx.lineTo(x, y + height - radius);
    ctx.quadraticCurveTo(x, y + height, x + radius, y + height);
    ctx.lineTo(x + width - radius, y + height);
    ctx.quadraticCurveTo(x + width, y + height, x + width, y + height - radius);
    ctx.lineTo(x + width, y + radius);
    ctx.quadraticCurveTo(x + width, y, x + width - radius, y);
    ctx.lineTo(x + radius, y);
    ctx.quadraticCurveTo(x, y, x, y + radius);
    ctx.stroke();
}

if (canvas.getContext) {
    let ctx = <CanvasRenderingContext2D> canvas.getContext('2d')
    ctx.beginPath();
    roundedRect(ctx, 12, 12, 150, 150, 15);
    roundedRect(ctx, 19, 19, 150, 150, 9);
    roundedRect(ctx, 53, 53, 49, 33, 10);
    roundedRect(ctx, 53, 119, 49, 16, 6);
    roundedRect(ctx, 135, 53, 49, 33, 10);
    roundedRect(ctx, 135, 119, 25, 49, 10);
   
    ctx.beginPath();
    ctx.arc(37, 37, 13, Math.PI / 7, -Math.PI / 7, false);
    ctx.lineTo(31, 37);
    ctx.fill();
    let i: number = 0;
    for (i = 0; i < 8; i++){
    ctx.fillRect(51 + i * 16, 35, 4, 4);
    }
   
    for (i = 0; i < 6; i++){
    ctx.fillRect(115, 51 + i * 16, 4, 4);
    }
   
    for (i = 0; i < 8; i++){
    ctx.fillRect(51 + i * 16, 99, 4, 4);
    }
   
    ctx.beginPath();
    ctx.moveTo(83, 116);
    ctx.lineTo(83, 102);
    ctx.bezierCurveTo(83, 94, 89, 88, 97, 88);
    ctx.bezierCurveTo(105, 88, 111, 94, 111, 102);
    ctx.lineTo(111, 116);
    ctx.lineTo(106.333, 111.333);
    ctx.lineTo(101.666, 116);
    ctx.lineTo(97, 111.333);
    ctx.lineTo(92.333, 116);
    ctx.lineTo(87.666, 111.333);
    ctx.lineTo(83, 116);
    ctx.fill();
   
    ctx.fillStyle = "white";
    ctx.beginPath();
    ctx.moveTo(91, 96);
    ctx.bezierCurveTo(88, 96, 87, 99, 87, 101);
    ctx.bezierCurveTo(87, 103, 88, 106, 91, 106);
    ctx.bezierCurveTo(94, 106, 95, 103, 95, 101);
    ctx.bezierCurveTo(95, 99, 94, 96, 91, 96);
    ctx.moveTo(103, 96);
    ctx.bezierCurveTo(100, 96, 99, 99, 99, 101);
    ctx.bezierCurveTo(99, 103, 100, 106, 103, 106);
    ctx.bezierCurveTo(106, 106, 107, 103, 107, 101);
    ctx.bezierCurveTo(107, 99, 106, 96, 103, 96);
    ctx.fill();
   
    ctx.fillStyle = "black";
    ctx.beginPath();
    ctx.arc(101, 102, 2, 0, Math.PI * 2, true);
    ctx.fill();
   
    ctx.beginPath();
    ctx.arc(89, 102, 2, 0, Math.PI * 2, true);
    ctx.fill();
}
```

我们不会很详细地讲解上面的代码，因为事实上这很容易理解。重点是绘制上下文中使用到了fillStyle属性，以及封装函数（例子中的roundedRect()）。使用封装函数对于减少代码量以及复杂度十分有用。

### Path2D 对象

正如我们在前面例子中看到的，你可以使用一系列的路径和绘画命令来把对象“画”在画布上。为了简化代码和提高性能，Path2D对象已可以在较新版本的浏览器中使用，用来缓存或记录绘画命令，这样你将能快速地回顾路径。

怎样产生一个Path2D对象呢？

Path2D()会返回一个新初始化的Path2D对象（可能将某一个路径作为变量——创建一个它的副本，或者将一个包含SVG path数据的字符串作为变量）。

```
new Path2D();     // 空的Path对象
new Path2D(path); // 克隆Path对象
new Path2D(d);    // 从SVG建立Path对象
```

所有的路径方法比如moveTo, rect, arc或quadraticCurveTo等，如我们前面见过的，都可以在Path2D中使用。

Path2D API 添加了 addPath作为将path结合起来的方法。当你想要从几个元素中来创建对象时，这将会很实用。比如：

添加了一条路径到当前路径（可能添加了一个变换矩阵）。

`Path2D.addPath(path [, transform])​`

#### example

在这个例子中，我们创造了一个矩形和一个圆。它们都被存为Path2D对象，后面再派上用场。随着新的Path2D API产生，几种方法也相应地被更新来使用Path2D对象而不是当前路径。在这里，带路径参数的stroke和fill可以把对象画在画布上。

```
let canvas = <HTMLCanvasElement> document.getElementById('tutorial')

if (canvas.getContext) {
    let ctx = <CanvasRenderingContext2D> canvas.getContext('2d')
    ctx.beginPath();
    let rectangle = new Path2D();
    rectangle.rect(10, 10, 50, 50);

    let circle = new Path2D();
    circle.moveTo(125, 35);
    circle.arc(100, 35, 25, 0, 2 * Math.PI);

    ctx.stroke(rectangle);
    ctx.fill(circle);
}
```

### 使用 SVG paths

新的Path2D API有另一个强大的特点，就是使用SVG path data来初始化canvas上的路径。这将使你获取路径时可以以SVG或canvas的方式来重用它们。

这条路径将先移动到点 (M10 10) 然后再水平移动80个单位(h 80)，然后下移80个单位 (v 80)，接着左移80个单位 (h -80)，再回到起点处 (z)。你可以在Path2D constructor 查看这个例子。

`var p = new Path2D("M10 10 h 80 v 80 h -80 Z");`

## 色彩

到目前为止，我们只看到过绘制内容的方法。如果我们想要给图形上色，有两个重要的属性可以做到：fillStyle 和 strokeStyle。

设置图形的填充颜色。

`fillStyle = color`

设置图形轮廓的颜色。

`strokeStyle = color`

color 可以是表示 CSS 颜色值的字符串，渐变对象或者图案对象。我们迟些再回头探讨渐变和图案对象。默认情况下，线条和填充颜色都是黑色（CSS 颜色值 #000000）。

```
let canvas = <HTMLCanvasElement> document.getElementById('tutorial')

if (canvas.getContext) {
    let ctx = <CanvasRenderingContext2D> canvas.getContext('2d')
    for (let i = 0; i < 6; i++) {
        for (let j = 0; j < 6; j++) {
            ctx.fillStyle = 'rgb(' + Math.floor(255 - 42.5 * i) + ',' +
                Math.floor(255 - 42.5 * j) + ',0)';
            ctx.fillRect(j * 25, i * 25, 25, 25);
        }
    }
}
```

## 文本

canvas 提供了两种方法来渲染文本:

在指定的(x,y)位置填充指定的文本，绘制的最大宽度是可选的.

`fillText(text, x, y [, maxWidth])`

在指定的(x,y)位置绘制文本边框，绘制的最大宽度是可选的.

`strokeText(text, x, y [, maxWidth])`

### example

```
let canvas = <HTMLCanvasElement> document.getElementById('tutorial')

if (canvas.getContext) {
    let ctx = <CanvasRenderingContext2D> canvas.getContext('2d')
    ctx.font = "24px serif";
    ctx.fillText("Hello world", 10, 50);
}
```

### 有样式的文本

在上面的例子用我们已经使用了 font 来使文本比默认尺寸大一些. 还有更多的属性可以让你改变canvas显示文本的方式：

当前我们用来绘制文本的样式. 这个字符串使用和 CSS font 属性相同的语法. 默认的字体是 10px sans-serif。

`font = value`

文本对齐选项. 可选的值包括：start, end, left, right or center. 默认值是 start。

`textAlign = value`

基线对齐选项. 可选的值包括：top, hanging, middle, alphabetic, ideographic, bottom。默认值是 alphabetic。

`textBaseline = value`

文本方向。可能的值包括：ltr, rtl, inherit。默认值是 inherit。

`direction = value`

如果你之前使用过CSS，那么这些选项你会很熟悉。

```
let canvas = <HTMLCanvasElement> document.getElementById('tutorial')

if (canvas.getContext) {
    let ctx = <CanvasRenderingContext2D> canvas.getContext('2d')
    ctx.font = "24px serif";
    ctx.textBaseline = "hanging";
    ctx.strokeText("Hello world", 10, 50);
}
```

## 动画

```
let sun = new Image();
let moon = new Image();
let earth = new Image();
function init(){
  sun.src = 'https://mdn.mozillademos.org/files/1456/Canvas_sun.png';
  moon.src = 'https://mdn.mozillademos.org/files/1443/Canvas_moon.png';
  earth.src = 'https://mdn.mozillademos.org/files/1429/Canvas_earth.png';
  window.requestAnimationFrame(draw);
}

function draw() {
  let canvas = <HTMLCanvasElement> document.getElementById('tutorial')
  let ctx = <CanvasRenderingContext2D> canvas.getContext('2d')
  ctx.globalCompositeOperation = 'destination-over';
  ctx.clearRect(0, 0, 300, 300); // clear canvas

  ctx.fillStyle = 'rgba(0,0,0,0.4)';
  ctx.strokeStyle = 'rgba(0,153,255,0.4)';
  ctx.save();
  ctx.translate(150, 150);

  // Earth
  let time = new Date();
  ctx.rotate( ((2 * Math.PI) / 60) * time.getSeconds() + ((2 * Math.PI) / 60000) * time.getMilliseconds() );
  ctx.translate(105, 0);
  ctx.fillRect(0, -12, 50, 24); // Shadow
  ctx.drawImage(earth, -12, -12);

  // Moon
  ctx.save();
  ctx.rotate( ((2 * Math.PI) / 6) * time.getSeconds() + ((2 * Math.PI) / 6000) * time.getMilliseconds() );
  ctx.translate(0, 28.5);
  ctx.drawImage(moon, -3.5, -3.5);
  ctx.restore();

  ctx.restore();
  
  ctx.beginPath();
  ctx.arc(150, 150, 105, 0, Math.PI * 2, false); // Earth orbit
  ctx.stroke();
 
  ctx.drawImage(sun, 0, 0, 300, 300);

  window.requestAnimationFrame(draw);
}

init();
```

