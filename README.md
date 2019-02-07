# mobile-web
移动web开发的一些知识点笔记,也是读书籍**移动web手册**的一些笔记

移动端web开发的三大主题：浏览器，视口，触摸事件。


## 视口

### meta标签
meta标签用于设置理想视口，把布局视口的尺寸设置成理想视口的大小，这就是响应式布局。
```html

<meta name = "viewport" content = "width= device-width,initial-scale = 1,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no">
```
解释一下上面的meta标签：
其中整个meta标签的设置是为了实现响应式布局，将布局视口尺寸设置为理想视口大小。

**width = device-width:设置布局视口的大小**: 可以将其设置为device-width,这样的话所有的布局视口尺寸始终与理想视口一样。

**initial-scale:初始缩放比例**，记住：**所有的缩放都是相对于理想视口的。**比如理想视口的宽度是375px,如果我们没有进行缩放，那么
一个元素的width:375px。将会正好占据整个屏幕，但是如果我们缩放2，那么这个元素将占据整个屏幕的一半。

**minimum-scale=1.0:最小缩放比例**，最小缩放比例是25%。安卓webkit不支持minimum-scale。都是相对于理想视口而言进行的缩放

**maximum-scale=1.0:最大缩放比例**，最大缩放比例为4。都是相对于理想视口而言进行的缩放

**user-scalable=no：禁止用户缩放**

注意：所有的缩放都是相对于理想视口进行的。比如，理想视口为375px,那么当元素宽度为375px时，会占满整个屏幕，
但是当缩放0.5的时候，只会占满半个屏幕，也就是它的实际宽度时375/2。


几个重要的尺寸获取：
document.documentElement.clientWidth/clientHeight:获取的是布局视口的尺寸。(被普遍支持，使用非常多)
window.innerWidth/innerHeight:返回视觉视口的尺寸。(接近被普遍支持，基本不使用)
screen.width/height:返回理想视口的尺寸。(几乎不被支持,基本不使用)


### 媒体查询

媒体类型查询：这是什么类型的设备。
视口相关的媒体查询：媒体查询的核心。(重要)
特性相关媒体查询：浏览器是否支持某某特性。(不重要)

媒体查询的基本格式：
```html

 @media all and (max-width:600px){
      #box{
        width:375px;
        height:200px;
        background: red;
      }
 }

```
上面的媒体查询表示只有在布局视口的宽度小于或者等于或600px的时候，css样式才会起作用

这里有很多的重要知识点：
0.媒体查询使用@media 关键字
1. 所有的媒体查询都需要媒体类型，通常使用all是最好的。
@media + 媒体名字 +{特定媒体下的CSS样式},可以实现特定媒体下展示特殊的CSS样式
```html
@media tv{
	#box{
		width:200px;
		height:200px;
		background:red;
	}
}

```
2. and是关键字，用来连接媒体类型和媒体特性。
and: 和,与。用来连接媒体设备和媒体特性。
not: 排除指定媒体类型。
only:指定某种特定的媒体类型。

```html
 @media tv and (max-width:400px){
      #box{
        width:375px;
        height:200px;
        background: green;
      }
 }

```
表示：只有在既是tv设备同时宽度小于等于400px的情况下，展示的样式。
3. min或者max一定要带括号,其实是媒体特性一定要加括号
4. 通常你应该在你的媒体查询中总是使用min-或者max-前缀。
   常见的媒体特性：
			 min-width:宽度大于设置值时进行识别
			 max-width:宽度小于设置值时进行识别
			 orientation:portrait         竖屏
			 orientation:landscape        横屏
			 -webkit-min-device-pixel-ratio:1.5  像素比为2
	```html
		@media (orientation:landscape){
				#box{
					width:200px;
					height:200px;
					background:green;
				}
		}
	```		 
5. width:600px 一定要带有单位。媒体查询的单位通常是像素，

**注意：媒体类型和媒体特性都可以单独使用，并不一定需要同时使用，他们只不过是CSS判断的条件罢了。**

媒体查询主要用于响应式，响应式与移动端不是同一回事。不要混为一谈。




### CSS样式

和桌面浏览器一样，移动浏览器也支持CSS，且支持的特性也差不多。但是
某些情况下移动浏览器对CSS样式的支持还是有必要不太一样的。
移动浏览器和桌面浏览器对CSS的支持不尽相同，有以下4个原因：
1. PC端的一些样式在移动端不存在，或者无法更好地适应触屏。比如：hover
2. 需要考虑视口时，规范中并没有说明是哪个视口(布局视口或视觉视口)。比如,单位vw和vh。
3. 对独立可滚动层的需求，这在各种资源受限制的移动设备上更难实现。比如：background-attachment。
4. 硬件限制,尤其是涉及内存和GPU的时候。比如过渡和动画在移动设备上可能无法使用。这个问题的根源在于设备而不是浏览器。


一些常见的在移动web中存在问题的CSS样式：

#### 1 position:fixed

在移动web开发的时候，我们通常会将顶部导航栏进行固定，不随页面进行滚动。
这时候，如果是在PC端可以直接使用position:fixed。但是在移动端不建议，或者说
不允许使用position:fixed。因为你不知道这里的盒是相对于哪个视口(布局视口还是视觉视口)
进行固定。


#### 2 overflow:auto

元素如果设置了overflow:auto。如果它超出了页面的宽度或者高度那么它应该会出现滚动条实现
滚动，**也就是说层必须可以独立滚动。**只有能够实现滚动，才能够看到超出隐藏的部分。
但是如果浏览器设置了不允许滚动(一些代理浏览器通常会这样设置)，那么就会导致overflow:auto。无法进行
滚动，也就看不到其他内容了。
总而言之：overflow:auto不适合于移动设备，虽然技术上可行，但是最好不要使用。

#### 3 background-attachment
 
 移动浏览器无法支持过多的滚动层，因此background-attachment在移动端同样是不可靠的，不建议使用。
 
#### 4 尺寸单位vw和vh

**vw和vh代表视口的百分比**，比如，50vw表示50的视口宽度，
20vh表示20%的视口高度。那么问题又来了，这里的视口究竟是
布局视口还是视觉视口了?考虑各种因素后确定应该是布局视口，
因为如果是视觉视口，每次用户缩放都会导致用户的宽高发生变化。
这样的设计是没有意义的。
这两个单位在移动端使用较少，目前主要是在响应式中进行使用。

#### 5 :active和:hover

这两个伪类在桌面环境中一直存在，但是被失败地移植到了触屏上。
不过，我们仍然可以放心地使用它，因为最坏情况也不过是他们无法
正常工作，不会出现其他后果。


### Flex布局
#### 父元素的设置：针对的是所有的元素整体。

Flex布局的一个关键点就是区分主轴和侧轴。我们平常进行开发时，页面展示都是在一个平面内，
平面的方向主要包括X和Y两个方向。而弹性盒模型的关键就是根据这个平面设置了主轴和侧轴两个参考轴,
根据这两个参考轴来进行关键的布局。因此学会弹性盒模型的关键就是一定要区分谁是主轴，谁是侧轴。
** 1.设置dispaly:flex。产生的效果。**
  如果设置了display:flex。那么所有的元素会沿着主轴排列，而且元素的宽度会动态计算。不会出现宽度不够的问题。
  
** 2. 设置主轴方向：flex-direction**
由于所有的元素都会直接沿着主轴排列，因此确定主轴的方向非常重要。flex-direction用于设置主轴方向。
```html
   flex-direction:row;
```
flex-direction:有多个值:
    row:表示从左往右(常见的X轴方向)
    row-reverse:表示从右往左
    column:表示从上往下
    column-reverse:表示从下往上。
不同的主轴设置，表示元素不同的排列方式。
**牢记一点：元素始终沿着主轴排列**

** 3. 主轴元素和富裕空间之间的排列方式**
当元素沿着主轴排列时，如果宽度不够，那么会动态计算每个元素的宽度。如果宽度有剩余，那么剩余空间和元素之间
有多种排列方式。justify-content:有多个值
```html
   justify-content:center;  所有主轴上的元素居中排列。
```
	flex-start:表示元素在主轴开始位置，富裕空间在主轴结束位置。
	flex-end:表示元素在主轴结束位置，富裕空间在主轴开始位置。
	center:表示元素在主轴中间，富裕空间在主轴两侧位置。
	space-between:富裕空间平均分配在每两个元素之间。
	space-around:富裕空间分配在每个元素两侧。
这里所有的设置都是以主轴上的元素作为主题。
比如justify-content:center。表示 主轴上的元素位于主轴的中间。justify-content:flex-start。表示主轴元素位于主轴开始的位置。


** 4. 设置元素在侧轴的排列** 
我们之前说过，flex布局区分主轴和侧轴。侧轴通过align-**来进行设置。
align-item:flex-start|flex-end|center|baselline|stretch
```html
   align-items:center;  设置侧轴元素的排列方式。
```
align-items:能够设置所有的元素在侧轴的排列方式(如果主轴为X方向，那么侧轴是Y方向，也就是垂直方向)


总结：
justify:表示主轴的排列。
align:表示侧轴的排列。
content:表示元素与富裕空间的管理。
justify-content:表示主轴元素与富裕空间的排列。
align-content:表示侧轴元素与富裕空间的排列(侧轴必须有两行以上)。关注的是换行元素的富裕空间的处理。


#### 子元素的设置：针对的是单个元素。

** 1. 设置子元素的排列顺序** 

这里所有的属性都是加在子元素身上，针对的是单个子元素。
order:改变单个子元素的顺序。 数字越大，越靠后。支持负数。

** 2. 设置子元素的大小-伸缩比例**

可以通过flex:设置元素占据的比例。
 
** 3. 设置子元素的侧轴对齐-针对的是某一个元素在侧轴的排列**
align-self:flex-start flex-end center。
align:表示侧轴。self:表示自己，即单个子元素。


## 移动端开发流程及其注意事项：

### 移动端开发注意事项：

1、设置动态像素比：是为了将设计稿尺寸于设备实现1:1还原。
```
   var pixelRatio = 1/window.devicePixelRatio;
   document.write('<meta name="viewport" content = "width=device-width,initial-scale='+pixelRatio+',minimum-scale='+pixelRatio+',maximum-scale='+pixelRatio+'"/>')
```
		

2.动态设置html字体大小,实现rem布局。是为了让布局在任何设备下都是以相同的百分比展示
```
   var html =document.getElementsByTagName('html')[0];
   var page = html.getBoundingClientRect().width;
   html.style.fontSize = page/16 +'px';
   
```
  
3.大致框架设计：也就是头部固定。
```
	html {
	  height: 100%;
	  overflow: hidden;
	}
	body {
	  height: 100%;
	  overflow: auto;
	}

```
4.图片的伸缩。我们虽然解决了布局随设备发生变化。但是图片的大小是固定的。如果不进行设置，那么默认是原始大小。
    我们必须保证图片也随设备大小变化而变化。通过设置background-size:xxrem。将图片的大小也设置成rem相关。
```
图片作为背景图片时，设置背景尺寸
background:url()
background-size: 0.825rem 1.375rem;
图片直接作为标签时，设置width:100%或者具体的width和hight。图片的width或者Height只要有一个设置了百分比，那么就会等比缩放。
width:100%;或者
width:xx/@rem;height:xx/@rem
```
5.固定定位元素中，如果有input元素，那么触发键盘之后，会导致固定定位错位。
  解决办法1：改变固定定位，使用绝对定位代替固定定位
  解决办法2：不使用input标签，而是使用div去模拟。  

6.关于字体，由于移动端和PC端字体显示有差异，因此，需要找到能够在PC端和移动端都显示的字体列表。
当不能进行显示时，能够切换到合适的字体。可以直接参考淘宝等网站。
```
font-family: Arial,Helvetica,STHeiTi,sans-serif;
```
7.碰到文字，一定要测量行高，不然会不准确。

