
# Flex布局

# 一、flex弹性盒子概念

弹性盒子是一种用于**按行或按列**布局元素的**一维**布局方法 。元素可以膨胀以填充额外的空间, 收缩以适应更小的空间。

而我们为什么需要使用flex布局呢，相较于position定位布局和float浮动布局，flex布局操作更加的灵活和简便，解决元素居中问题， 自动弹性伸缩，适配不同大小的屏幕，和适配移动端。

# 二、flex模型

如果一个元素设置了**display:flex**(即开启了flex布局)，那么它就是这个弹性盒子的**flex container**,其直接子元素叫做**flex items**。

当元素表现为flex盒子是，它们沿着两个轴布局：

![](https://obs-pic-1309372570.cos.ap-chongqing.myqcloud.com/20220921211620.png)


flex布局是用于按行或按列布局元素当布局方法，其中设定有**主轴main axis**,**交叉轴cross axis**(垂直于主轴)，它们代表着flex元素排列的方向，比如为flex盒子设置主轴flex-direction:row(默认值)，即该弹性盒子横向排列，所以主轴为横向的行，交叉轴为纵向的列。

两条轴有起始线(main/cross-start)与结束线（main/cross-end)。

# 三、flex布局方法

## flex-container
![](https://obs-pic-1309372570.cos.ap-chongqing.myqcloud.com/20220921211656.png)

html代码结构:

``` html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Document</title>
	<style>
		#flexbox{
			display: flex;
			background-color: aqua;
			width: 600px;
			height: 600px;
			flex-flow: row wrap;
			align-items: center;
		}
		.item{
			width: 120px;
			height: 100px;
			background-color: antiquewhite;
			border: 3px solid black;
			margin : 1px;
			font-size: 30px;
		}
	</style>
</head>
<body>
	<div id='flexbox'>
		<div class="item">1</div>
		<div class="item">2</div>
		<div class="item">3</div>
		<div class="item">4</div>
		<div class="item">5</div>
		<div class="item">6</div>
		<div class="item">7</div>
		<div class="item">8</div>
		<div class="item">9</div>
	</div>
</body>
</html>
```

## flex-direction 设置主轴方向

|参数|主轴方向|
|-:     |-:|
|row(默认值)|左⇒右|
|column|上⇒下	 |
|row-reverse| 右⇒左|
|column-reverse	|下⇒上 |

![](https://obs-pic-1309372570.cos.ap-chongqing.myqcloud.com/20220921212019.png)


## flex-wrap 主轴 行满是否换行

![](https://obs-pic-1309372570.cos.ap-chongqing.myqcloud.com/20220921212040.png)

![](https://obs-pic-1309372570.cos.ap-chongqing.myqcloud.com/20220921212051.png)

## flex-flow

## flex-flow

flex-direction 和 flex-wrap的结合体
`flex-low: [flex-direction] [flex-wrap]`
![](https://obs-pic-1309372570.cos.ap-chongqing.myqcloud.com/20220921212120.png)

## justify-content 主轴元素对齐方式

![](https://obs-pic-1309372570.cos.ap-chongqing.myqcloud.com/20220921212136.png)

![](https://obs-pic-1309372570.cos.ap-chongqing.myqcloud.com/20220921212154.png)

之上所有操作都是只对于主轴,要想达到完全效果，还有接下来的交叉轴元素对齐设置。

## align-items item在交叉轴上的对齐方式*

![](https://obs-pic-1309372570.cos.ap-chongqing.myqcloud.com/20220921212209.png)
![](https://obs-pic-1309372570.cos.ap-chongqing.myqcloud.com/20220921212220.png)

## align-content 交叉轴行对齐方式 多行

![](https://obs-pic-1309372570.cos.ap-chongqing.myqcloud.com/20220921212236.png)
![](https://obs-pic-1309372570.cos.ap-chongqing.myqcloud.com/20220921212258.png)

## flex-items
![](https://obs-pic-1309372570.cos.ap-chongqing.myqcloud.com/20220921212310.png)

## flex-grow

![](https://obs-pic-1309372570.cos.ap-chongqing.myqcloud.com/20220921212333.png)

## flex-shrink

![](https://obs-pic-1309372570.cos.ap-chongqing.myqcloud.com/20220921212346.png)

## flex-basis

![](https://obs-pic-1309372570.cos.ap-chongqing.myqcloud.com/20220921212403.png)

## order

![](https://obs-pic-1309372570.cos.ap-chongqing.myqcloud.com/20220921212412.png)

## align-self 自定义某个item的对齐方式

![](https://obs-pic-1309372570.cos.ap-chongqing.myqcloud.com/20220921212423.png)

# 四、总结

相信看到这里，你对flex布局已经了解了不少，本文只是对常用对flex布局对一些属性做了简单对介绍，对于更多flex相关的知识可以移步[MDN文档flexBox](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Flexbox)了解更多！