# mobile-web
移动web开发的一些知识点笔记

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
2. and是逻辑运算符与&  前后一定要有空格 ,逗号表示或
3. min或者max一定要带括号
4. 通常你应该在你的媒体查询中总是使用min-或者max-前缀。
5. width:600px 一定要带有单位。媒体查询的单位通常是像素，


