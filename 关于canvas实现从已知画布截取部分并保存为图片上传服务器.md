#####关于canvas实现从已知画布截取部分并保存为图片上传服务器

今天分享的是业务中遇到的一个技术问题，提炼为关于canvas实现从已知画布截取部分并保存为图片上传服务器

###### canvas背景知识补充：

> ```
> 1.<canvas> 标签只有两个属性—— width和height。这些都是可选的，并且同样利用 DOM properties 来设置。
> 当开始时没有为canvas规定样式规则，其将会完全透明，也就是说最终转化保存的图片是透明背景色的。
> 
> 2.canvas更有意思的一项特性就是图像操作能力。可以用于动态的图像合成或者作为图形的背景，以及游戏界面（Sprites）等等。浏览器支持的任意格式的外部图片都可以使用，比如PNG、GIF或者JPEG。 你甚至可以将同一个页面中其他canvas元素生成的图片作为图片源。
> ```

##### 1.裁剪画布

在最近的项目中，我遇到了一个问题，原本我通过计算知道了截取初始位置，宽和高，想着怎么修改插件源码让canvas只绘制我想要的部分，发现源码并不好改，牵扯内容太多，于是**可以将同一个页面中其他canvas元素生成的图片作为图片源**canvas这个技术点便成了关键，以已知画布canvas1作为图片源，新建一个新的画布，将所要的目标图片部分重绘到新画布即可

关键代码原理

```javascript
ctx.drawImage(img, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight);
//dx, dy, dWidth, dHeight,表示在canvas画布上规划处一片区域用来放置图片，dx, dy为canvas元素的左上角坐标，dWidth, dHeight指canvas元素上用在显示图片的区域大小

//sx,sy,swidth,sheight,这4个坐标是针对图片元素的，表示图片在canvas画布上显示的大小和位置。sx,sy表示图片上sx,sy这个坐标作为左上角，然后往右下角的swidth,sheight尺寸范围图片作为最终在canvas上显示的图片内容。
```

代码实现如下：

```javascript
//计算得到画布canvasX(宽)，canvasY（高），startX（起始位置横坐标），startY（起始位置纵坐标）,已知画布canvas1
var canvas = document.createElement('canvas');
var ctx = canvas.getContext('2d');
canvas.width = canvasX;
canvas.height = canvasY;
ctx.drawImage(canvas1, startX, startY, canvasX, canvasY, 0, 0, canvasX, canvasY);
document.body.appendChild(canvas);//查看重汇的canvas
```

##### 2.之后我要将截取的部分图canvas上传至服务器,利用canvas.toBlob方法即可实现

关键代码语法:

```javascript
void canvas.toBlob(callback, type, encoderOptions);
//callback:回调函数，可获得一个单独的Blob对象参数。
//type 可选 DOMString类型，指定图片格式，默认格式为image/png。
```

业务代码如下：

```javascript
canvas.toBlob(function (blob) {
   console.log(blob)
   var form = new FormData();
  	form.append('image', blob, 'a.png');
    api.uploadImage(form).then(data => {
        console.log(data.url);
     });

 })
```

参考文档：<https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API>