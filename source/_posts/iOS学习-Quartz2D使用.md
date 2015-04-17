title: "iOS学习-Quartz2D使用"
date: 2015-03-31 21:29:47
tags: Quartz2D使用
---
###一、什么是Quartz2D?
1. Quartz2D是一个二维绘图引擎，同时支持IOS和Mac系统,基于路径的绘制
2. Quartz2D能做成的工作
3. 绘制图形：线条、三角形，矩形，圆，弧等
4. 绘制文字
5. 绘制/生成图片（图像）
6. 读取/生成PDF
7. 截图/裁剪图片
8. 自定义UI控件。（当我们不满足系统给出的控件，可以自己制造）
9. `Quartz2D的API是纯C语言的，Quartz2D的API基于core Graphics框架`
 
###二、Quartz2D是怎样将绘图信息和绘图的属性绘制到图形上下文中去的呢？
`说明：` 	
- 新建一个项目，自定义一个view类和storyboard关联后，重写该类中的drawRect：方法,drawRect方法当View第一次显示到屏幕上时会被调用，以及调用setNeedsDisplay或者setNeedsDisplayInRectangular时   
+ 划线的三个步骤：
 
 1.  获取上下文
 2.	绘制路径
 3.	渲染

####Paths

1. paths中的重要元素
*Point（点）*

```
	void CGContextMoveToPoint (
	    CGContextRef c,
	    CGFloat x,
	    CGFloat y
	 );
	 指定一个点成为当前点（current point），Quartz会更重current point，执行完一个相关函数后，current point会相应的改变到函数结束位置 
```
*Lines（线段）*

相关的几个函数
```
	 void CGContextAddLineToPoint (
	    CGContextRef c,
	    CGFloat x,
	    CGFloat y
	 );
	 创建一条直线，从current point到 (x,y)
	 然后current point会变成(x,y)
	 
	 void CGContextAddLines (
	    CGContextRef c,
	    const CGPoint points[],
	    size_t count
	 );
	 创建多条直线，比如points有两个点，那么会画两条直线 从current point到 (x1,y1),
	 然后是(x1,y1)到(x2,y2)
	 然后current point会变成points中的最后一个点
	 
	 //设置线段的宽度
	 CGContextSetLineWidth(CGContextRef c, CGFloat width)
	
	//设置线段头尾部的样式(但是Xcode更新后好多效果都差不多)
	CGContextSetLineCap(CGContextRef c, CGLineCap cap)//第二个参数是枚举值
	
	//设置线段转折点的样式
	CGContextSetLineJoin(CGContextRef c, CGLineJoin join)//同上
	
	//设置点划线的样式
    const CGFloat lengths[]={2,6,4,1};
    CGContextSetLineDash(ctx, 0, lengths, 4);
	CGContextSetLineDash(CGContextRef c, CGFloat phase, const CGFloat *lengths, size_t count)
```
*Arcs（弧线）*

1. 创建弧线之弧度法
	 假如想创建一个完整的圆圈，那么 开始弧度就是0 结束弧度是 2pi， 因为圆周长是 2*pi*r.
	 最后，函数执行完后，current point就被重置为(x,y).
	 还有一点要注意的是，假如当前path已经存在一个subpath，那么这个函数执行的另外一个效果是有一条直线，从current point到弧的起点
	 ```
	 void CGContextAddArc (
	    CGContextRef c,    
	    CGFloat x,             //圆心的x坐标
	    CGFloat y,    //圆心的y坐标
	    CGFloat radius,   //圆的半径 
	    CGFloat startAngle,    //开始弧度
	    CGFloat endAngle,   //结束弧度（360°是2* M_PI不是M_2_PI）
	    int clockwise          //0表示顺时针，1表示逆时针
	 );
	 ```
2. 创建弧线之切线法
	原理：首先画两条线，这两条线分别是 current point to (x1,y1) 和(x1,y1) to (x2,y2).
	 这样就是出现一个以(x1,y1)为顶点的两条射线，
	 然后定义半径长度，这个半径是垂直于两条射线的，这样就能决定一个圆了，更好的理解看下图，不过个人认为下图所标的 tangent point 1的位置是错误的。
	 最后，函数执行完后，current point就被重置为(x2,y2).
	 
	 ```
	 void CGContextAddArcToPoint (
	    CGContextRef c,
	    CGFloat x1,  //端点1的x坐标
	    CGFloat y1,  //端点1的y坐标
	    CGFloat x2,  //端点2的x坐标
	    CGFloat y2,  //端点2的y坐标
	    CGFloat radius //半径
	 );
	 ```
	 	 
*Curves(曲线)*
定义几个控制点，切着控制点组成的直线而形成的曲线
	1. 三次曲线函数
	```
	void CGContextAddCurveToPoint (
		    CGContextRef c,
		    CGFloat cp1x, //控制点1 x坐标
		    CGFloat cp1y, //控制点1 y坐标
		    CGFloat cp2x, //控制点2 x坐标
		    CGFloat cp2y, //控制点2 y坐标
		    CGFloat x,  //曲线的终点 x坐标
		    CGFloat y  //曲线的终点 y坐标
		 );
	```
	2. 二次曲线函数
	```
	void CGContextAddQuadCurveToPoint (
	    CGContextRef c,
	    CGFloat cpx,  //控制点 x坐标
	    CGFloat cpy,  //控制点 y坐标
	    CGFloat x,  //曲线的终点 x坐标
	    CGFloat y  //曲线的终点 y坐标
	  );
	```
	
*Clip（剪辑）*
`裁剪原理`
	自定义一个View，在这个View中勾画出你要裁剪的区域，然后再将图片盖到裁剪区域的上方，指定裁剪的地方显示出来，其他地方不显示，这就是裁剪的原理。

---

####例子：
- 要求：绘制颜色为红色的直线，利用的颜色空间设置颜色
- `注意：`颜色空间设置颜色必须在绘制完成之后对其进行回收
- 代码：

```
- (void)drawRect:(CGRect)rect
{
    // Drawing code
    // 1. 获取绘图上下文
    CGContextRef ctx = UIGraphicsGetCurrentContext();
    
    // 2. 设置绘图状态
    // 设置线宽
    CGContextSetLineWidth(ctx, 1);
    
    // 设置颜色
    // 创建颜色空间
    CGColorSpaceRef colorSpace = CGColorSpaceCreateDeviceRGB();
    const CGFloat components[] = {1,0,0,1};
    // 创建颜色
    CGColorRef color = CGColorCreate(colorSpace, components);
    // 设置描边的颜色
    CGContextSetStrokeColorWithColor(ctx, color);
    
    // 设置点划线
//    const CGFloat lengths[] = {2,6,4,1};
//    CGContextSetLineDash(ctx, 0, lengths, 4);
    
    // 3. 构建Path
    NSLog(@"%@", NSStringFromCGPoint(rect.origin));
    // 移动画笔到指定点
    // 开始path
    CGContextBeginPath(ctx);
    CGContextMoveToPoint(ctx, rect.origin.x, rect.origin.y);
    
    // 添加一条直线
    CGContextAddLineToPoint(ctx, CGRectGetMaxX(rect), CGRectGetMaxY(rect));
    
    // 关闭path
    CGContextClosePath(ctx);
    
    // 4. 绘制 stroke fill
//    CGContextFillPath(<#CGContextRef c#>)
//    CGContextStrokePath(<#CGContextRef c#>)
    CGContextDrawPath(ctx, kCGPathStroke);
    
    // 5. 回收资源
    CGColorRelease(color);
    CGColorSpaceRelease(colorSpace);
}
```

- 效果图：
![image](/images/Quartz2D/Line.png)

- 要求：绘制矩形区域，以红线描边，绿色填充
- 代码：

```
- (void)drawRect:(CGRect)rect
{
    // 获取上下文
    CGContextRef ctx = UIGraphicsGetCurrentContext();
    // 设置描边颜色
    CGContextSetStrokeColorWithColor(ctx, [UIColor redColor].CGColor);
    // 设置填充颜色
    CGContextSetFillColorWithColor(ctx, [UIColor greenColor].CGColor);
    // 设置线宽
    CGContextSetLineWidth(ctx, 3);
    // 添加矩形区域
    CGContextAddRect(ctx, rect);
    
//    CGContextStrokePath(ctx);
	// 绘制区域以及线
    CGContextDrawPath(ctx, kCGPathFillStroke);
}
```
- 效果图：
![image](/images/Quartz2D/Rect.png)

- 要求：绘制弧线，以切线方法，最终绘制为一个直径为100的圆
- 代码：

```
- (void)drawRect:(CGRect)rect
{
    // 获取上下文
    CGContextRef ctx = UIGraphicsGetCurrentContext();
    // 设置描边颜色
    CGContextSetStrokeColorWithColor(ctx, [UIColor greenColor].CGColor);
    
    CGFloat x = rect.origin.x;
    CGFloat y = 100;
    // 移动画笔到第一个切点(以rect顶点为原点)
    CGContextMoveToPoint(ctx, x, y);
    // 确定两条切线交点位置，以及另一个切点位置
    //CGContextAddArcToPoint(CGContextRef c, CGFloat x1, CGFloat y1, CGFloat x2, CGFloat y2, CGFloat radius)
    // c代表上下文，x1为交点x坐标，y1为交点y坐标，x2为第二切点的x，y2为第二切点的y，radius为弧线半径
    CGContextAddArcToPoint(ctx, x, y+100, x+100, y+100, 100);
    CGContextAddArcToPoint(ctx, x+200, y+100, x+200, y, 100);
    CGContextAddArcToPoint(ctx, x+200, y-100, x+100, y-100, 100);
    CGContextAddArcToPoint(ctx, x, y-100, x, y, 100);
    // 绘制
    CGContextStrokePath(ctx);
}
```
- 效果图：
![image](/images/Quartz2D/Arc.png)

- 要求：绘制贝塞尔曲线
- 代码：

```
- (void)drawRect:(CGRect)rect
{
	// 获取上下文
    CGContextRef ctx = UIGraphicsGetCurrentContext();
    
    CGContextSetStrokeColorWithColor(ctx, [UIColor blackColor].CGColor);
    
    CGFloat x = 0;
    CGFloat y = 0;
    
    CGContextMoveToPoint(ctx, x, y);
    
    // CGContextAddCurveToPoint(CGContextRef c, CGFloat cp1x, CGFloat cp1y, CGFloat cp2x, CGFloat cp2y, CGFloat x, CGFloat y)
    // cp1x，cp1y代表第一个贝塞尔参考点位置x和y，最后的xy代表终点位置
    CGContextAddCurveToPoint(ctx, x+150, y+50, x+50, y+350, x+200, y+400);
   //CGContextAddArcToPoint(ctx, x+150, y+50, x+200, y, 100);
//    CGContextAddArcToPoint(ctx, x+200, y-100, x+100, y-100, 100);
//    CGContextAddArcToPoint(ctx, x, y-100, x, y, 100);
    
    CGContextStrokePath(ctx);
}
```

- 效果图：
![image](/images/Quartz2D/curve.png)

###三、UIBezierPath类
1. 使用UIBezierPath类可以创建基于矢量的路径。此类是Core Graphics框架关于path的一个封装。使用此类可以定义简单的形状，如椭圆或者矩形，或者有多个直线和曲线段组成的形状。
2. UIBezierPath对象是CGPathRef数据类型的封装。path如果是基于矢量形状的，都用直线和曲线段去创建。我们使用直线段去创建矩形和多边形，使用曲线段去创建弧（arc），圆或者其他复杂的曲线形状。每一段都包括一个或者多个点，绘图命令定义如何去诠释这些点。每一个直线段或者曲线段的结束的地方是下一个的开始的地方。每一个连接的直线或者曲线段的集合成为subpath。一个UIBezierPath对象定义一个完整的路径包括一个或者多个subpaths。
3. 创建和使用一个path对象的过程是分开的。创建path是第一步，包含一下步骤：
	1. 创建一个path对象。
	2. 使用方法moveToPoint:去设置初始线段的起点。
	3. 添加line或者curve去定义一个或者多个subpaths。
	4. 改变UIBezierPath对象跟绘图相关的属性。例如，我们可以设置stroked path的属性lineWidth和lineJoinStyle。也可以设置filled path的属性usesEvenOddFillRule。
- 要求：绘制圆角矩形，以及绘制不带箭尾的箭
- 代码：

```
- (void)drawRect:(CGRect)rect
{
    // Drawing code
    // 绘制圆角矩形
    // 1. 创建bezier path
    UIBezierPath *path = [UIBezierPath bezierPathWithRoundedRect:rect byRoundingCorners:UIRectCornerAllCorners cornerRadii:CGSizeMake(100, 50)];
    // 2. 设置填充颜色
    [[UIColor greenColor] setFill];
    
    // 3. 填充
    [path fill];
    /**
     *  绘制不带箭尾的箭
     *  UIBezierPath
     */
    
    // 1. 黑色的箭shaft(轴)
    UIBezierPath *p = [UIBezierPath bezierPath];
    [p setLineWidth:20];
    [p moveToPoint:CGPointMake(100, 100)];
    [p addLineToPoint:CGPointMake(100, 25)];
    [[UIColor blackColor] setStroke];
    [p stroke];
    
    // 2. 红色的箭头(arrow)
    [[UIColor redColor] setFill];
    [p moveToPoint:CGPointMake(80, 25)];
    [p addLineToPoint:CGPointMake(100, 0)];
    [p addLineToPoint:CGPointMake(120, 25)];
    [p fill];
}
```
- 效果图：
![image](/images/Quartz2D/bezierpath.png)

- 要求：绘制渐变色箭尾的箭
- 代码：
```
- (void)drawRect:(CGRect)rect
{
    // 坐标系转换(坐标原点转换到（80，0）位置)
    CGContextTranslateCTM(ctx, 80, 0);
    
    // > 设置箭尾的三角区为不绘制区域(clipping)
    // 保存初始状态
    CGContextSaveGState(ctx);
    
    CGContextMoveToPoint(ctx, 10, 100);
    CGContextAddLineToPoint(ctx, 20, 90);
    CGContextAddLineToPoint(ctx, 30, 100);
    // CGContextGetClipBoundingBox自动绘制一个包含上述三角形的矩形区域
    CGRect flipRect = CGContextGetClipBoundingBox(ctx);
    
    CGContextAddRect(ctx, flipRect);
    // 以奇偶填充法填充
    CGContextEOClip(ctx);

    // > 绘制箭杆
    CGContextSetStrokeColorWithColor(ctx, [UIColor blackColor].CGColor);
    CGContextSetLineWidth(ctx, 20);
    
    CGContextMoveToPoint(ctx, 20, 100);
    CGContextAddLineToPoint(ctx, 20, 25);
    // CGContextReplacePathWithStrokedPath将线转换为矩形区域
    CGContextReplacePathWithStrokedPath(ctx);
    
    CGContextClip(ctx);
    
//    CGContextStrokePath(ctx);
    
    // 给shaft添加渐变
    
    CGColorSpaceRef colorSpace = CGColorSpaceCreateDeviceRGB();
    // 设置RGB以及Alpha
    const CGFloat components[] = {0.6, 0.6, 0.6, 0.6,
                                    0.0, 0.0, 0.0, 1.0,
                                    0.6,0.6,0.6,0.6};
    // 设置渐变位置
    const CGFloat locations[] = {0, 0.618, 1};
    
    CGGradientRef gradient = CGGradientCreateWithColorComponents(colorSpace, components, locations, 3);
    CGContextDrawLinearGradient(ctx, gradient, CGPointMake(10, 100), CGPointMake(30, 100), 0);
    // 回到初始状态
    CGContextRestoreGState(ctx);
    // 回收颜色空间
    CGColorSpaceRelease(colorSpace);
    
    // > 箭头
    CGContextSetFillColorWithColor(ctx, [UIColor redColor].CGColor);
    
    CGContextMoveToPoint(ctx, 0, 25);
    CGContextAddLineToPoint(ctx, 20, 0);
    CGContextAddLineToPoint(ctx, 40, 25);
    
    CGContextFillPath(ctx);
}
```
- 效果图：
![image](/images/Quartz2D/arrowWithGradient.png)



	 

