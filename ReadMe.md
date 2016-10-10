
## Canvas Notes

Canvas 总结

####目录

* Basic usage

	获取canvas渲染上下文`getContext()`,对于2D图像,使用`getContext('2d')`

	```JavaScript
    var canvas = document.getElementById('canvas');
    if (canvas.getContext){
      var ctx = canvas.getContext('2d');
    }
	```

* Drawing shapes

	**绘制矩形**

	绘制矩形的三个函数,x,y表征矩形左上角相对canvas原点的坐标.

	```JavaScript
	fillRect(x,y,width,height)     //填充矩形
	strokeRect(x,y,width,height)   //绘制框线
	clearRect(x,y,width,height)    //清除矩形区域,全透明
	```

	**绘制path**

	path是一系列点通过线段或曲线相连构成的图形,可以闭合或开口.

	相关函数:
	```JavaScript	
	beginPath()                    //创建一个新的path,后续绘图指令添加到path中	
	closePath()                    //结束该path,闭合图形,后续绘图指令回归到context中
	stroke()                       //绘制框线
	fill()                         //填充区域
	```
	绘图函数:
	```JavaScript
	moveTo(x,y)                    //移动画笔位置到(x,y)坐标

	lineTo(x,y)					   //当前位置到(x,y)绘制直线

	arc(x,y,radius,startAngle,endAngle,anticlockwise)  //绘制一个圆弧,圆心在(x,y),半径为radius,起始角度为startAngle,终止角度为endAngle,是否为逆时针)

	arcTo(x1, y1, x2, y2, radius)  //绘制弧线

	//由当前位置绘制二次贝塞尔曲线到(x,y),控制点坐标为(cp1x,cp1y).
	quadraticCurveTo(cp1x, cp1y, x, y)

	//由当前位置绘制贝塞尔曲线到(x,y),两个控制点坐标为(cp1x,cp1y)与(cp2x,cp2y).
	bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)

	//绘制矩形到当前path中
	rect(x,y,width,height)

	//Path2D图形
	new Path2D();

	```


* Applying styles and colors
	
	添加颜色,线型,渐变,模式,阴影.

	**Colors颜色**	
	```JavaScript

	//填充颜色
	fillStyle = color

	//线框颜色
	strokeStyle = color

	```

	其中color是css<color>或者一个渐变对象或者一个pattern对象.

	```JavaScript
	ctx.fillStyle = "orange";
	ctx.fillStyle = "#FFA500";
	ctx.fillStyle = "rgb(255,165,0)";
	ctx.fillStyle = "rgba(255,165,0,1)";	
	```

	**全局透明度globalAlpha**

	```JavaScript
	globalAlpha = transparencyValue    //后续绘制的shape使用该透明度
	```

	**线型**

	```JavaScript
	//设置线宽度
	lineWidth = value

	//设置线段末端类型,三种可选类型[butt,round,square]
	lineCap = type

	//设置线段连接点类型,三种可选类型[round,bevel,miter]
	lineJoin = type

	//设置虚线
	setLineDash([line_dist, gap_dist]);
	lineDashOffset = offset

	```   

	**渐变**

	渐变对象可以赋值给fillStyle与strokeStyle

	首先定义CanvasGradient对象,两种方法.

	```JavaScript

	//线性渐变,起始点(x1,y1),终止点(x2,y2)
	createLinearGradient(x1,y1,x2,y2)

	//径向渐变,起始圆(x1,y1,r1),终止圆(x2,y2,r2)
	createRadialGradient(x1,y1,r1,x2,y2,r2)

	```

	然后使用addColorStop(position,color)设定颜色,position在[0,1]之间,相对位置.


	**样式Pattern**

	```JavaScript

	//image是`CanvasImageSouce`,可以为HTMLImageElement,另一个Canvas,<video>
	//type可选[repeat,repeat-x,repeat-y,no-repeat]
	createPattern(image,type)

	```


	**阴影**

	阴影设置包含如下4个属性:

	```JavaScript
	//水平偏移量
	shadowOffsetX = float

	//垂直偏移量
	shadowOffsetY = float

	//模糊大小
	shadowBlur = float

	//阴影颜色
	shadowColor = color
	```

* Drawing text

	**绘制text**

	Canvas提供两种方法进行绘制:

	```JavaScript
	//在(x,y)填充绘制text,可选绘制最大宽度
	fillText(text, x, y [, maxWidth])

	//在(x,y)边框绘制text,可选绘制最大宽度
	strokeText(text, x, y [, maxWidth])
	```

	**text样式**

	使用如下属性

	```JavaScript
	//设置字体font,与CSS font属性相同
	font = value

	//value可选[start,end,left,right,center]
	textAlign = value

	//value可选[top,hanging,middle,alphabetic,ideographic,bottom]
	textBaseline = value

	//value可选[ltr,rtl,inherit]
	direction = value
	```

* Using images

	**获取图片**

	Canvas可以使用的图片来源包含如下:

	* HTMLImageElement

	通过Image()或者<img>创建的图片

	* HTMLVideoElement

	获取<video>的当前帧画面

	* HTMLCanvasElement

	使用另一个Canvas

	**绘制图片**

	* drawImage(image,x,y)                    

	绘制指定的图片.

	* drawImage(image, x, y, width, height)

	将图片缩放到指定大小

	* drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)

	截取图片左上(sx,sy),宽度高度为(sWidth,sHeight)的部分绘制到canvas上左上(dx,dy)宽度高度为(dWidth,dHeight)的部分

* Transformations

	**状态的保存于恢复**

	使用`save()`与`restore()`方法,Canvas状态存储在栈中，每当save()方法被调用后，当前的状态就被推送到栈中保存.每一次调用 restore 方法，上一个保存的状态就从栈中弹出.

	**移动**

	```JavaScript
	translate(x,y)  //移动原点到(x,y)	
	```

	**旋转**

	```JavaScript
	rotate(angle)   //以原点为中心顺时针旋转(rad),使用translate改变旋转中心
	```

	**缩放**

	```JavaScript
	scale(x, y)     //x,y 分别是横轴和纵轴的缩放因
	```

* Compositing and clipping

	**globalCompositeOperation**

	默认将图像绘制在另一个之上,利用globalCompositeOperation可以在已有图形后面画新图形,还可以用来遮盖,清除某些区域.

	```JavaScript
	globalCompositeOperation = type //type有12种取值
	```
	* source-over       默认
	* destination-over  在原内容之下绘制
	* source-in			新图形与原图形重叠部分显示，其余透明
	* destination-in    原图形与新图形重叠部分显示，其余透明
	* source-out		新图形与原图形不重叠部分显示，其余透明
	* destination-out	原图形与新图形不重叠部分显示，其余透明
	* source-atop		新图形中与原有内容重叠的部分会被绘制，并覆盖于原有内容之上
	* destination-atop  原有内容中与新内容重叠的部分会被保留，并会在原有内容之下绘制新图形
	* lighter			两图形中重叠部分作加色处理
	* darker			两图形中重叠的部分作减色处理
	* xor				重叠的部分会变成透明

	**裁切路径 Clipping paths**

	裁切路径和普通的 canvas 图形差不多，不同的是它的作用是遮罩，用来隐藏没有遮罩的部分.它可以实现与 source-in 和 source-atop 差不多的效果。最重要的区别是裁切路径不会在 canvas 上绘制东西，而且它永远不受新图形的影响

	```JavaScript
	clip()  //创建剪裁路径,与fill()和stroke()同类
	```

* Basic animations

	**动画的基本步骤**

	* 清空 canvas, 使用clearRect()方法
	* 保存 canvas 状态
	* 绘制动画图形（animated shapes）,重绘动画帧,可以用window.setInterval(), window.setTimeout(),和window.requestAnimationFrame()来设定定期执行一个指定函数
	* 恢复 canvas 状态


* Pixel manipulation

	**ImageData 对象**

	包含三个只读属性:
	* width
	* height
	* data

	```JavaScript
		//创建imageData对象
		var myImageData = ctx.createImageData(width, height);
		//得到场景像素数据
		var myImageData = ctx.getImageData(left, top, width, height);
		//另存为图片
		canvas.toDataURL('image/jpeg', quality)
	```

* Optimizing the canvas

	* 在离屏canvas上预渲染相似的图形或重复的对象
	* 避免浮点数的坐标点，用整数取而代之
	* 不要在用drawImage时缩放图像
	* 使用多层画布去画一个复杂的场景
	* 用CSS设置大的背景图
	* 用CSS transforms特性缩放画布

