


# 前端整理合集
- [前端整理合集](#前端整理合集)
  - [html](#html)
    - [如何理解html语义化](#如何理解html语义化)
    - [p标签里面不能嵌套ul、div等块级元素原因](#p标签里面不能嵌套uldiv等块级元素原因)
  - [css](#css)
    - [offsetWidth](#offsetwidth)
    - [margin负值](#margin负值)
    - [BFC](#bfc)
      - [margin重叠](#margin重叠)
    - [三栏布局](#三栏布局)
    - [粘连布局](#粘连布局)
    - [清除浮动](#清除浮动)
    - [盒模型](#盒模型)
    - [响应式设计](#响应式设计)
    - [元素水平垂直居中](#元素水平垂直居中)
    - [line-height继承](#line-height继承)
    - [rem em vw vh dpr](#rem-em-vw-vh-dpr)
      - [移动端1px实现](#移动端1px实现)
      - [移动端2X3X图](#移动端2x3x图)
    - [css预处理语言](#css预处理语言)
    - [flex布局](#flex布局)
    - [元素竖向的百分比设定是相对于容器的高度吗？](#元素竖向的百分比设定是相对于容器的高度吗)
    - [css选择器](#css选择器)
      - [css选择器读取顺序](#css选择器读取顺序)
      - [可继承](#可继承)
    - [css元素隐藏](#css元素隐藏)
    - [css画三角形](#css画三角形)
    - [css视差滚动实现方案](#css视差滚动实现方案)
    - [3D立体感绕x轴旋转](#3d立体感绕x轴旋转)
      - [transform-style立体交叉遮盖](#transform-style立体交叉遮盖)
    - [css性能优化](#css性能优化)
  - [js](#js)
    - [window.getComputedStyle(element) 获取伪类中的内容](#windowgetcomputedstyleelement-获取伪类中的内容)
    - [js中哪些会被判断为false](#js中哪些会被判断为false)
    - [js 类型转换机制](#js-类型转换机制)
    - [Promise](#promise)
    - [== 和 ===](#-和-)
      - [==的注意之处](#的注意之处)
      - [\[\]==!\[\] {}==!{}的结果](#-的结果)
    - [深拷贝和浅拷贝](#深拷贝和浅拷贝)
    - [数据类型](#数据类型)
      - [undefined和null的区别](#undefined和null的区别)
      - [判断数据类型的方法](#判断数据类型的方法)
    - [数据结构](#数据结构)
      - [数组和链表的应用场景](#数组和链表的应用场景)
    - [字符串常用的方法](#字符串常用的方法)
      - [match matchAll](#match-matchall)
      - [substring slice字符串处理区别](#substring-slice字符串处理区别)
    - [数组常用的方法](#数组常用的方法)
      - [影响到原数组的方法](#影响到原数组的方法)
    - [event loop](#event-loop)
      - [event loop 宏任务 微任务 和dom渲染的关联](#event-loop-宏任务-微任务-和dom渲染的关联)
    - [Class和实例的关系以及原型链](#class和实例的关系以及原型链)
      - [函数的特殊性](#函数的特殊性)
    - [js如何实现继承](#js如何实现继承)
      - [继承的方案](#继承的方案)


## html

### 如何理解html语义化
1.	让人更容易读懂（增加代码可读性）
2.	去掉或丢失样式的时候能够让页面呈现出清晰结构
3.	让搜素引擎更容易理解（SEO优化）

### p标签里面不能嵌套ul、div等块级元素原因
不符合语义化规定，w3c规定，对于P元素，它指定了以下内容，这表明P元素只允许包含内联元素（包括p元素本身也不行）。
```
<!ELEMENT P - O (%inline;)* -- paragraph -->
```
简而言之，不可能在DOM中放置`<div>`元素，因为开放的`<div>`标签会自动closures`<p>`元素。
```js
<p>
    11
    <div href='www.baidu.com'>baidu1</div>
    22
</p>
```

![p标签](book_files/1.jpg)



## css

### offsetWidth
offsetWidth 属性是一个只读属性,返回一个元素的布局宽度.一个典型的（译者注：各浏览器的 offsetWidth `可能有所不同`）.offsetWidth = border + padding + scrollbar(竖直方向滚动条) + width.

![offsetWidth](book_files/2.jpg)

```html
<!DOCTYPE html>
<html>
<head>
    <title>盒模型</title>
    <style type="text/css">
        #div1 {
            width: 100px;
			height:50px;
            padding: 10px;
            border: 1px solid #ccc;
            margin: 10px;
			overflow-y:auto ;
            /* box-sizing: border-box; */
        }
    </style>
</head>
<body>
    <div id="div1">
        this is div1
		this is div1
		this is div1
		this is div1
		this is div1
    </div>
    <script>
 		console.log(document.getElementById('div1').offsetWidth) // 122
		let div1 = document.getElementById('div1')
		// 使用 getComputedStyle 获取元素的计算后样式  
		var style = window.getComputedStyle(div1); 
		console.log(style.paddingLeft) // 10px
		console.log(style.paddingRight)// 10px
		console.log(style.width) //83.2px // 滚动条占据了一定的宽度
		console.log(style.marginLeft)//10px
		console.log(style.marginRight)//10px
    </script>
</body>
</html>
```
> offsetWidth 注意不同盒模型时结果，IE盒模型给的width已经包含了boder padding 故就是100


### margin负值
1.	margin-top和margin-left为负值时，元素向上或者向左移动
2.	margin-bottom为负值时，下方元素上移，自身不受影响
3.	margin-right为负值时，右侧元素左移，自身不受影响(margin-right负值也可以理解为自身在越来越小，当不占width又是float时可以浮动上去，三栏布局可应用到)

```html
<!DOCTYPE html>
<html>
<head>
    <title>margin 负值</title>
    <style type="text/css">
        body {
            margin: 20px;
        }

        .float-left {
            float: left;
        }
        .clearfix:after {
            content: '';
            display: table;
            clear: both;
        }

        .container {
            border: 1px solid #ccc;
            padding: 10px;
        }
        .container .item {
            width: 100px;
            height: 100px;
        }
        .container .border-blue {
            border: 1px solid blue;
            margin-right:-10px;
        }
        .container .border-red {
            border: 1px solid red;
        }
    </style>
</head>
<body>
    
    <p>用于测试 margin top bottom 的负数情况</p>
    <div class="container">
        <div class="item border-blue">
            this is item 1
        </div>
        <div class="item border-red">
            this is item 2
        </div>
    </div>

    <p>用于测试 margin left right 的负数情况</p>
    <div class="container clearfix">
        <div class="item border-blue float-left">
            this is item 3
        </div>
        <div class="item border-red float-left">
            this is item 4
        </div>
    </div>

</body>
</html>
```

![margin负值](book_files/9.jpg)


k1和k2是两个div
1. 上下布局
   + k1 margin-top 负值，k1往上移动，k2跟随移动对应距离
   + k2 margin-top 负值，k1不动，k2往上移动，层级比k1高，可覆盖点击不到K1的事件
   + k1 margin-bottom 负值 k1不动 k2往上移动
   + k2 margin-bottom 负值 k2不动，k2后如果有相邻的元素比如k3存在，则会向上移动
2. 左右布局
   + k1 margin-left 负值 k1往左移动，k2跟随移动对应距离
   + k2 margin-left 负值 k1不动，k2 往左移动，层级比k1高
   + k1 margin-right 负值 k1不动 k2往左移动
   + k2 margin-right 负值 k2不动 k2后如果有相邻的元素比如k3存在，则会向左移动

实际应用：`圣杯布局+粘连布局`

### BFC
在页面中元素都有一个隐含的属性叫作`Block Formatting Context`，即块级格式化上下文，简称BFC。该属性能够设置打开或关闭，默认是关闭的。页面上的一个`隔离渲染区域`，**容器里面的子元素不会在布局上影响到外面的元素**

一旦开启元素的BFC后，元素将会具备如下特性：

+ 父元素的垂直`外边距`不会和子元素重叠
+ 开启BFC的元素`不会被浮动元素`所覆盖
+ 开启BFC的元素能够包含浮动的子元素


1. 普通文档流布局规则
	+ 浮动的元素是不会被父级计算高度
	+ 非浮动元素会占据浮动元素的位置
	+ margin会传递给父级[根据规范，一个盒子如果没有上补白和上边框，那么它的上边距应该和其文档流中的第一个孩子元素的上边距重叠。]
	+ 两个相邻元素上下margin会重叠
2. BFC布局规则
	+ 浮动的元素会被父级计算高度（父级触发了BFC）
	+ `非浮动元素不会占据浮动元素位置（非浮动元素触发了BFC）`
	+ margin不会传递给父级（父级触发了BFC）
	+ 两个相邻元素上下margin会重叠（给其中一个元素增加一个父级，然后让他的父级触发）【margin重叠三个条件:同属于一个BFC;相邻;块级元素】
3. 产生方式
	+ float 不为none
	+ overflow不为visible
	+ position不为relative和static
	+ display为table-cell table-caption `inline-block`之一
	+ 根元素HTML
4. BFC作用 :多栏布局,清除浮动,上下margin重叠

> 多栏布局指的是左侧float，然后给整体父级加上BFC模式，这样就可以避免右侧的内容占据左侧

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <title></title>
        <style>
            *{margin:0;padding:0}
            .k1{
                width:100px;
                height:100px;
                border:1px solid #00D6B2;
                float:left;
                
            }
            .k2{
                width:200px;
                height:200px;
                background-color: bisque;
                /* overflow: hidden; */
                
            }
        </style>
    </head>
    <body>
        <div class="k1">
            
        </div>
        <div class="k2">
            abc def weq1 abcdef weq1
            abc def weq1 abcdef weq1
            abc def weq1 abcdef weq1
            abc def weq1 abcdef weq1
            abc def weq1 abcdef weq1
            abc def weq1 abcdef weq1
            abc def weq1 abcdef weq1
            abc def weq1 abcdef weq1
            abc def weq1 abcdef weq1
            abc def weq1 abcdef weq1
        </div> 
    
    </body>
</html>
```

![图一](book_files/4.jpg)

放开k2中/* overflow: hidden; */

![图二](book_files/5.jpg)


```html
<style>
  div {
    border: solid 1px red;
    width: 50px;
    height: 50px;
  }
  .float {
    float: left;
  }
</style>
<div>1</div>
<div class="float">2</div>
<div class="float">3</div>
<div style='background: red;'>4</div>
<div style='background: blue;'>5</div>
<div class="float">6</div>
```

如果给2设置上背景色且不透明，那么就看不到4的背景色red的样式了，相当于2脱离了文档流，浮动在4的上方了。

![图片](book_files/6.jpg)
![图片](book_files/7.jpg)
![图片](book_files/8.jpg)

#### margin重叠
margin-top和margin-bottom重叠，空白p被忽略，所以最后相距`15px`
```html
<!DOCTYPE html>
<html>
<head>
    <style type="text/css">
        p {
            font-size: 16px;
            line-height: 1;
            margin-top: 10px;
            margin-bottom: 15px;
        }
    </style>
</head>

<body>
    <p>AAA</p>
    <p></p>
    <p></p>
    <p></p>
    <p>BBB</p>
</body>
</html>
```

![图例](book_files/3.jpg)


### 三栏布局
+ flex grid
+ absolute + margin
+ 圣杯布局：center left right在同一层级全部浮动，父级设左右padding，左侧：left:-100% + right对应宽度，右边margin-right对应宽度 
+ 双飞翼布局：多一个div包裹，中间div用margin不是padding，左侧不需要再借助定位改变位置，右侧不需要通过margin-right直接使用margin-left即可

```html
<style>
*{
    margin:0;
    padding:0
}
#container {
  padding-left: 200px; 
  padding-right: 150px;
}
#container .column {
  float: left;
}

#center {
  width: 100%;
  background:greenyellow
}

#left {
  width: 200px; 
  margin-left: -100%;
  position: relative;
  right: 200px;
  background: skyblue;
}
#right {
  width: 150px; 
  margin-right: -150px; 
  background: mediumvioletred;
}

#footer {
  clear: both;
}

</style>
<div id="header">header</div>
<div id="container">
  <div id="center" class="column">1</div>
  <div id="left" class="column">2</div>
  <div id="right" class="column">3</div>
</div>
<div id="footer">footer</div>
```

![1](book_files/10.jpg)

div内容为2的位置因为浮动本应该在1之后，然后设置margin-left：-100%；相当于向左移动了整个#container位置，还需要在通过定位移动200px

![2](book_files/11.jpg)

div内容为3的位置，设置了margin-right负值，本来应该影响右侧内容，但是它的右侧没内容，假设有，右侧内容会慢慢左移，当margin-right负值等于div3的大小，就相当于完全遮盖住，**外界感觉div3相当于没了宽度，没了宽度的div3自然可以移动上去。**

![3](book_files/12.jpg)

双飞翼布局
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>双飞翼布局</title>
    <style type="text/css">
        body {
            min-width: 550px;
        }
        .col {
            float: left;
        }

        #main {
            width: 100%;
            height: 200px;
            background-color: #ccc;
        }
        #main-wrap {
            margin: 0 190px 0 190px;
        }

        #left {
            width: 190px;
            height: 200px;
            background-color: #0000FF;
            margin-left: -100%;
        }
        #right {
            width: 190px;
            height: 200px;
            background-color: #FF0000;
            margin-left: -190px;
        }
    </style>
</head>
<body>
    <div id="main" class="col">
        <div id="main-wrap">
            this is main
        </div>
    </div>
    <div id="left" class="col">
        this is left
    </div>
    <div id="right" class="col">
        this is right
    </div>
</body>
</html>

```

### 粘连布局
1. 为内容区域添加最小的高度
	+ min-height
	+ padding-bottom top元素
	+ margin-top bottom元素(margin负值的应用)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
		*{
			margin: 0;
			padding: 0;         
		}
		html,body{
			height: 100%;
		}
		#main{
			min-height: 100%;  
			background-color: orange;
		}
		.main{
			padding-bottom: 100px;
		}
		#footer{
			height: 100px;
		    background: pink;
			margin-top: -100px;
		}
    </style>
</head>
<body>
    <div id="main">
        <div class="main">
            main<br>
            main<br>
            main<br>
            main<br>
            main<br>
            main<br>	
        </div>
    </div>
    <div id="footer">
        footer
    </div>
</body>
</html>
```

2. flex布局：footer的flex设为0，这样footer获得其固有的高度;content的flex设为1

### 清除浮动
1.	overflow:hidden
2.	父级设置固定高度
3.	clear:both;兼容性好，需要一个空div，语义化不好
4.	万能清除法

```css
.类名:after {
  content: "";
  clear: both;
  display: block;
  height: 0;
  overflow: hidden;
  visibility: hidden;
  zoom:1;
}
```

```html
<style type="text/css">
	.parent::after {  
	    content: ""; /* 必须设置内容，即使它是空的 */  
	    display: table; /* 创建一个匿名表格块级盒子 */  
	    clear: both; /* 清除浮动 */  
	}  
	.parent{
		border:3px slateblue solid 
	}
	.float-child {  
	    float: left; /* 或者使用 float: right; */  
	    width: 100px;  
	    height: 100px;  
	    background-color: lightblue;  
	    margin-right: 10px; /* 如果是左浮动的话 */  
	}
</style>
<div class="parent">  
    <div class="float-child">我是浮动元素我是浮动元素我是浮动元素我是浮动元素我是浮动元素我是浮动元素我是浮动元素</div>  
    <!-- 注意这里没有额外的清除标签 -->  
</div>
```
![img](book_files/13.jpg)


### 盒模型
盒模型指的是HTML元素在渲染时所占据的空间，包括元素的内容（content）、内边距（padding）、边框（border）和外边距（margin）。

```
box-sizing: content-box(标准盒模型)|border-box(IE盒模型)|inherit:
```


### 响应式设计
适配不同尺寸屏幕

+ 媒体查询 @media
+ 百分比
+ vw/vh
+ rem

### 元素水平垂直居中
+ position 定位四个方向值一致，margin:auto
+ position + transform
+ position + margin负值(需知道宽高)
+ grid
+ flex
+ table

```css
.father {  
	display: flex;  
	justify-content: center; /* 水平居中 */  
	align-items: center; /* 垂直居中 */  
}  
```
```css
	.father {
		display: table-cell;
		vertical-align: middle;
		text-align: center; 
	}
	.son {
		display: inline-block;
	}
```

```html
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>水平垂直居中方案</title>
    <style type="text/css">
        .container {
            border: 1px solid #ccc;
            margin: 10px;
            padding: 10px;
            height: 200px;
        }
        .item {
            background-color: #ccc;
        }
		 /*行内元素*/
        .container-1{
            text-align: center;
            line-height: 200px;
            height: 200px;
        }

        .container-2 {
            position: relative;
        }
		 /*需要知道宽高*/
        .container-2 .item {
            width: 300px;
            height: 100px;
            position: absolute;
            left: 50%;
            margin-left: -150px;
            top: 50%;
            margin-top: -50px;
        }

        .container-3 {
            position: relative;
        }
		/*不需要知道宽高，但是不兼容低版本浏览器*/
        .container-3 .item {
            width: 200px;
            height: 80px;
            position: absolute;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%)
        }

        .container-4 {
            position: relative;
        }
		 /*比较优秀的处理方案*/
        .container-4 .item {
            width: 100px;
            height: 50px;
            position: absolute;
            top: 0;
            left: 0;
            bottom: 0;
            right: 0;
            margin: auto;
        }
 		.container-5 {
           position: relative;
          justify-content: center;
          align-items: center;
          display:flex;
          
        }
        .container-5 .item {
            width: 100px;
            height: 50px;
        }

    </style>
</head>
<body>
    <div class="container container-1">
        <span>一段文字</span>
    </div>

    <div class="container container-2">
        <div class="item">
            this is item
        </div>
    </div>

    <div class="container container-3">
        <div class="item">
            this is item
        </div>
    </div>

    <div class="container container-4">
        <div class="item">
            this is item
        </div>
</div>
<div class="container container-5">
        <div class="item">
            this is item
        </div>
    </div>

</body>
</html>

```

### line-height继承
1.	写具体数值，如30px，则继承父级该值
2.	写比例如1/2/3.5等,则继承该比例（**自己的**font-size*父级中的比例）
3.	写百分比,如200%,则继承计算出来的结果(**父级**的font-size*200%)

### rem em vw vh dpr
1.	rem: 相对大小，但相对的只是HTML根元素
2.	em:  继承父级元素的字体大小
3.	vw:window.innerWidth = 100vw
4.	vh:window.innerHeight = 100vh
5.	vmax:取vh/vw中大值
6.	vmin: 取vh/vw中小
7.	dpr（设备像素比）：是指`设备物理像素的个数`除以`设备独立像素`的大小。物理像素是手机屏幕上一个一个的发光的点，大小是固定的；独立像素也叫做逻辑像素，css设置的像素大小就是逻辑像素。

`window.devicePixelRatio`可获取，无缩放的情况下，1个css像素 === 一个设备独立像素

![dpr](book_files/20.jpg)

#### 移动端1px实现
+  border-image:需要图片
+  background-image：因为每个边框都是线性渐变颜色实现，因此无法实现圆角。
+  box-shadow:不好控制
+  媒体查询：兼容性
+  :after transform (其实无非是把1px缩放为0.5px，**0.5px并不是所有都支持**(iOS8以上支持)。)

```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
<meta charset="UTF-8">  
<meta name="viewport" content="width=device-width, initial-scale=1.0">  
<title>1px Border with Transform and After</title>  
<style>  
  .border-1px {  
    position: relative;  
    background-color: white;  
  }  
  .border-1px::after {  
    content: "";  
    position: absolute;  
    left: 0;  
    top: 0;  
    width: 200%;  
    height: 200%;  
    border: 1px solid #000;  
    transform: scale(0.5);  
    transform-origin: 0 0;  
    box-sizing: border-box;  
    pointer-events: none; /* 防止影响点击事件 */  
  }  
</style>  
</head>  
<body>  
<div class="border-1px" style="width: 200px; height: 100px;">  
  使用transform和伪元素的1px边框  
</div>  
</body>  
</html>
```

+  viewport + rem

```html
<meta name="viewport" id="WebViewport" content="initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no">
```
```js
var viewport = document.querySelector("meta[name=viewport]")
if (window.devicePixelRatio == 1) {
    viewport.setAttribute('content', 'width=device-width, initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no')
} 
if (window.devicePixelRatio == 2) {
    viewport.setAttribute('content', 'width=device-width, initial-scale=0.5, maximum-scale=0.5, minimum-scale=0.5, user-scalable=no')
} 
if (window.devicePixelRatio == 3) {
    viewport.setAttribute('content', 'width=device-width, initial-scale=0.333333333, maximum-scale=0.333333333, minimum-scale=0.333333333, user-scalable=no')
} 
var docEl = document.documentElement;
var fontsize = 10 * (docEl.clientWidth / 320) + 'px';
docEl.style.fontSize = fontsize;
```
+ svg，postcss-write-svg(小插件，只适合画直线)

#### 移动端2X3X图
+ srcset

```html
<img srcset="my-image@1x.png 1x, my-image@2x.png 2x, my-image@3x.png 3x"  
     src="my-image@1x.png"  
     alt="My Image"  
     style="width:100%;height:auto;">
```
+ 媒体查询 兼容性差，目前之余IOS8+才支持，在IOS7及其以下、安卓系统都是显示0px。

```css
.my-element {  
  /* 默认背景图像，用于1倍像素密度的设备 */  
  background-image: url('my-image@1x.png');  
}  
  /* 针对2倍像素密度的设备 */
@media (-webkit-min-device-pixel-ratio: 2), (min-resolution: 192dpi) { 
  .my-element {  
    background-image: url('my-image@2x.png');  
  }  
}  
  
	/* 针对3倍像素密度的设备 */  
@media (-webkit-min-device-pixel-ratio: 3), (min-resolution: 288dpi) {  
  .my-element {  
    background-image: url('my-image@3x.png');  
  }  
}
```

+ js处理

```js
function setAppropriateImageSrc() {  
  var image = document.getElementById('my-image');  
  var dpr = window.devicePixelRatio || 1;  
  var src;  
  
  if (dpr >= 3) {  
    src = 'my-image@3x.png';  
  } else if (dpr >= 2) {  
    src = 'my-image@2x.png';  
  } else {  
    src = 'my-image@1x.png';  
  }  
  
  image.src = src;  
}  
  
window.onload = setAppropriateImageSrc;  
// 如果需要监听窗口大小变化，可以添加以下事件监听器  
window.onresize = setAppropriateImageSrc;
```

### css预处理语言
扩充css语言，增加了变量，混合，函数，嵌套，代码模块化等功能，方便复用和开发

+ 嵌套 scss less stylus

```css
.a {
	&.b {
		color: red;
	}
}
```
+ 变量处理

```less
// less要求变量@开头
@red : #c00;
strong {
	color: @red;
}
```
```scss
/* sass变量$开头 */
$red: #c00;
strong {
	color: $red;
}
```
```css
/* stylus变量可以$也可以不加符号，不要用@ */
$mainColor = #0982c1  
$siteWidth = 1024px  
borderStyle = dotted  
  
body  
  color $mainColor  
  border 1px borderStyle $mainColor  
  max-width $siteWidth
```

+ 作用域概念:和js类似，但是最好不要去定义重复名称变量，除非真的需要

> sass测试也一样，有的人说显示unscoped 会是white，实际测试不是
```less
@color: black;
.scoped {
 @bg: blue;
 @color: white;
 color: @color;
 background-color:@bg;
}
.unscoped {
 color:@color;
}
```
```css
.scoped {
  color: #ffffff;
  background-color: #0000ff;
}
.unscoped {
  color: #000000;
}
```

+ 混入(mixins)是预处理器最精髓之一，抽离提取。

在less中，混合的用法是指将定义好的classA引入classB中，同时还可以传参和带有默认参数
```less
.alert {
 font-weight: 700;
}
// 函数一样
.highlight(@color: red) {
 font-size: 1.2em;
 color: @color;
}
.heads-up {
 .alert;
 .highlight(red);
}
```
```css
.alert {
  font-weight: 700;
}
.heads-up {
  font-weight: 700;
  font-size: 1.2em;
  color: #ff0000;
}

```

sass需要关键词@mixin和引入时@include
```scss
@mixin large-text {
 font: {
 family: Arial;
 size: 20px;
 weight: bold;
 }
 color: #ff0000;
}
.page-title {
 @include large-text;
 padding: 4px;
 margin-top: 10px;
}
```
```scss
// 定义一个带参数的mixin  
@mixin large-text($family, $size, $weight) {  
  font-family: $family;  
  font-size: $size;  
  font-weight: $weight;  
  color: #ff0000;  
}  
  
// 使用mixin，并传递参数  
.page-title {  
  @include large-text(Arial, 40px, bold);  
  padding: 4px;  
  margin-top: 10px;  
}
```
```css
.page-title {
  font-family: Arial;
  font-size: 40px;
  font-weight: bold;
  color: #ff0000;
  padding: 4px;
  margin-top: 10px; }
```
stylus可以不加关键字,参数和函数写法一致，用等号
```css
error(borderWidth= 2px) {
 border: borderWidth solid #F00;
 color: #F00;
}
.generic-error {
 padding: 20px;
 margin: 4px;
 error(); /* error mixins */
}
.login-error {
 left: 12px;
 position: absolute;
 top: 20px;
 error(5px); /* error mixins $borderWidth 5px */
}
```

+ 其他逻辑判断

```scss
@for $i from 1 through 3 {  
  .item-#{$i} { width: 100px * $i; }  
}
$type: monster;  
body {  
  @if $type == monster {  
    background: green;  
  } @else if $type == demon {  
    background: red;  
  } @else {  
    background: blue;  
  }  
}
```
```css
.item-1 {
  width: 100px; }
.item-2 {
  width: 200px; }
.item-3 {
  width: 300px; }
body {
  background: green; }
```
stylus语法
```css
for i in 1..3  
  .item-{i}  
    width: 100px * i
type = 'monster'  
body  
  if type == 'monster'  
    background: green  
  else if type == 'demon'  
    background: red  
  else  
    background: blue
```
```css
.item-1 {  
  width: 100px;  
}  
.item-2 {  
  width: 200px;  
}  
.item-3 {  
  width: 300px;  
}
body {  
  background: green;  
}
```



### flex布局
+ flex-direction: 设置主轴的方向
+ **justify-content**: `设置主轴上的子元素排列方式`
+ flex-wrap: 设置子元素是否换行
+ align-content: 设置侧轴的子元素的排列方式（多行）
+ **align-items**:`设置侧轴上的子元素排列方式（单行）`
+ align-self:`允许单个项目有与其他项目不一样的对齐方式`，可覆盖align-items属性。
+ flex-flow:复合属性，相当于同时设置了flex-direction 和 flex-wrap
+ order：定义项目的排列顺序。数值越小，排列越靠前，默认为0。

```css
.box {
 justify-content: flex-start | flex-end | center | space-between | space-around;
}
```
+ space-between：两端对齐，项目之间的间隔都相等。
+ space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。

![1](book_files/14.jpg)

```css
.box { align-items: flex-start | flex-end | center | baseline | stretch; }
```

![2](book_files/15.jpg)

+ stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。

```css
.box {
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```
![3](book_files/16.jpg)

+ align-content属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。

```css
.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```
![4](book_files/17.jpg)

+ align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。



### 元素竖向的百分比设定是相对于容器的高度吗？
当按百分比设定一个元素的宽度时，它是相对于父容器的宽度计算的，但是，对于一些表示竖向距离的属性，例如 `padding-top , padding-bottom , margin-top , margin-bottom` 等，当按百分比设定它们时，依据的也是父容器的`宽度`，而不是高度。

### css选择器
id选择器 标签选择器 类选择器 后代选择器 子选择器(div>p) 相邻选择器(a+div) 群组选择器(div,p 选择所有div和p) 伪类选择器 伪元素选择器 属性选择器 层级选择器(p~ul 选择前面有p元素的所有ul)

#### css选择器读取顺序
CSS选择器的解析是从右向左解析的。若从左向右的匹配，发现不符合规则，需要进行回溯，会损失很多性能。

若从右向左匹配，先找到所有的最右节点，对于每一个节点，向上寻找其父节点直到找到根元素或满足条件的匹配规则，则结束这个分支的遍历。

两种匹配规则的性能差别很大，是因为从右向左的匹配在第一步就筛选掉了大量的不符合条件的最右节点（叶子节点），而从左向右的匹配规则的性能都浪费在了失败的查找上面。

#### 可继承
+ 可继承的属性：font-size, font-family, color
+ 不可继承的样式：border, padding, margin, width, height
+ 注意a标签不继承父级的`color`，h1-h6不继承font-size(是按照一定的em比例呈现的)

### css元素隐藏
+ display:none
+ visibility:didden
+ opacity:0
+ 宽高设为0
+ 定位出可视区
+ clip-path

![对比](book_files/18.jpg)

### css画三角形
可以看到,设置不同颜色的各个边框，就会发现边框是梯形，极限情况下，width height为0，其他边框颜色留一个边框就可以得到三角形

![三角形](book_files/19.jpg)


### css视差滚动实现方案
+ perspective  transform: translateZ() scale();
+ background-attachment

![perspective](book_files/22.jpg)

translateZ 值调节元素在 Z 轴的位置（近大远小），同时配合 scale 值让元素的大小看起来和原来无异。那么就实现了滚动过程中，不同元素看起来的运动速度不同。

```css
.container {
  width: 100vw;
  height: 100vh;
  overflow-x: auto;
  overflow-y: hidden;
  perspective: 1px;
}
.img-1 {
  transform: translateZ(-1px) scale(2); //变慢两倍
}
.img-2 {
  transform: translateZ(-2px) scale(3); //变慢三倍
}
.text-1 {
  transform: translateZ(0.5px) scale(0.5); //变快两倍
}

```


### 3D立体感绕x轴旋转
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8"> 
<title></title> 
<style>
#div1
{
	position: relative;
	height: 150px;
	width: 150px;
	margin: 50px;
	padding:10px;
	border: 1px solid black;
	perspective:150;
	-webkit-perspective:150; /* Safari and Chrome */
}

#div2
{
	padding:50px;
	position: absolute;
	border: 1px solid black;
	background-color: red;
	transform: rotateX(50deg);
	
}
</style>
</head>

<body>

<div id="div1">
  <div id="div2">HELLO</div>
</div>
 
</body>
</html>
```

![案例](book_files/21.jpg)

#### transform-style立体交叉遮盖
transform-style 属性指定嵌套元素是怎样在三维空间中呈现。
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8"> 
<title></title>
<style>
#div5
{
	position: relative;
	height: 200px;
	width: 200px;
	margin: 100px;
	padding:10px;
	border: 1px solid black;
}

#div6
{
	padding:50px;
	position: absolute;
	border: 1px solid black;
	background-color: red;
	transform: rotateY(60deg);
	transform-style: preserve-3d;
	-webkit-transform: rotateY(60deg); 
	-webkit-transform-style: preserve-3d;
}

#div7
{
	padding:40px;
	position: absolute;
	border: 1px solid black;
	background-color: yellow;
	transform: rotateY(-60deg);
	-webkit-transform: rotateY(-60deg); 

}

</style>
</head>

<body>

<div id="div5">
  <div id="div6">HELLO
  	<div id="div7">YELLOW</div>
  </div>
</div>

</body>
</html>
```

![图](book_files/23.jpg)


### css性能优化
1. 内联首屏`关键css`，可下载完html立刻渲染，不需要外链下载再渲染，但是不能缓存且代码不能过多(阻塞)
2. 异步加载css
```js
//方案1
const myCSS = document.createElement( "link" );
myCSS.rel = "stylesheet";
myCSS.href = "mystyles.css";
// header
document.head.insertBefore( myCSS, document.head.childNodes[ document.head.
childNodes.length - 1 ].nextSibling );
```
```html
<!-- 方案2： media设置noexist，再onload改为all，这样一开始不加载，等页面加载完毕后再去解析css -->
<link rel="stylesheet" href="mystyles.css" media="noexist" onload="this.med
ia='all'">
```
```html
<!-- 方案3：先rel配置可选，后加载完删除可选alternate -->
<link rel="alternate stylesheet" href="mystyles.css" onload="this.rel='styl
esheet'">
```
3. 资源压缩 webpack gulp
4. 选择器(从右往左执行，不要写太多层级div #x 直接写 #x更快)，尽量不用通配符和属性选择器
5. 减少昂贵属性使用 border-radius filter :nth-child box-shadow,重绘时会降低浏览器性能
6. 少使用@import

## js

### window.getComputedStyle(element) 获取伪类中的内容
1. 返回一个对象，可得到元素的所有 CSS 属性的值。私有的 CSS 属性值可以通过对象提供的 API 或通过简单地使用 CSS 属性名称进行索引来访问。

```js
let div1 = document.getElementById('div1')
// 使用 getComputedStyle 获取元素的计算后样式  
var style = window.getComputedStyle(div1); 
console.log(style.paddingLeft) // 10px
```

2. 与伪元素一起使用:getComputedStyle 可以从**伪元素拉取样式信息**

```css
 .k1::after {
    content: "rocks!";
  }
```

```js
let k1=document.getElementsByClassName("k1")[0]
console.log(window.getComputedStyle(k1, ':before').getPropertyValue("content")) //rocks!
//	console.log(window.getComputedStyle(k1, ':before').content)也可以这样写
```

### js中哪些会被判断为false
以下这些都会： 0 null undefined NaN ""
```js
if (0) {  
  console.log("This will not be logged because 0 is falsy.");  
}  
  
if (null) {  
  console.log("This will not be logged because null is falsy.");  
}  
  
if (undefined) {  
  console.log("This will not be logged because undefined is falsy.");  
}  
  
if (NaN) {  
  console.log("This will not be logged because NaN is falsy.");  
}  
  
if ("") {  
  console.log("This will not be logged because an empty string is falsy.");  
}
```

### js 类型转换机制
+ 显式转换： Number() parseInt() Boolean() String()...
+ 隐式转换: 根据运行的代码自动做了某些转换

```js
console.warn(Number('1a'))//NaN
console.warn(Number(true))//1
const obj ={
	toString:()=>{
		return 112
	}
}
console.log(Number(obj))// 112
console.log(parseInt('22a1'))//22
//console.log(Number(Symbol(1)))// Cannot convert a Symbol value to a number
console.log(Number(null))//0
console.log(Number([3,4]))//NaN
console.log(Number([3]))//3
const arr=[1,2,3]
arr.toString=function(){
	return 1111
}
console.log(Number(arr))//1111
```

```js
+new Date()// 转换成时间戳
null+1 //1
undefined+1//NaN
```

### Promise
+ 三种状态 pending resolved rejected
+ then函数正常返回resolved，里面有报错返回rejected[如果有返回值，那么对应的then或者catch能拿到]
+ catch函数正常返回resolved，里面有报错返回rejected[同上]

```js
Promise.resolve().then(() => {
    console.log(1) //1
    }).catch(() => {
        console.log(2)
    }).catch(() => {
        console.log(2)
}).catch(() => {
        console.log(2)
    }).then((val) => {
    console.warn(val)//undefined
        console.log(3)//3
})
```

```js
Promise.resolve().then(() => { // 返回 rejected 状态的 promise
    console.log(1)
    throw new Error('erro1')
}).catch((e) => { // 返回 resolved 状态的 promise
  console.log(e)  //Error: erro1
    console.log(2) // 2
}).catch(() => { 
    console.log(2)
}).catch(() => { 
    console.log(2)
}).then(() => {
    console.log(3)//3
})
```

### == 和 ===
全等不会存在隐式转换问题

#### ==的注意之处
两等操作符存在隐式转换的情形。

```js
let obj = {valueOf:function(){return 1}}
let result1 = (obj == 1); // true
console.log(result1)//true

console.log(NaN == NaN) //false

let obj1 = {toString:function(){return 1}}
console.log(obj1 == 1)//true

// 运算操作时，valueOf的优先级高于toString
let obj2 = {toString:function(){return 1},valueOf:function(){return 2}}
console.log(obj2 == 1)//false
```

两等操作符总结：
1. 两个都为简单类型，字符串和布尔值都会转换成**数值**，再比较
2. 简单类型与引用类型比较，**对象转化成其原始类型的值**，再比较
3. 两个都为引用类型，**则比较它们是否指向同一个对象**
4. null 和 undefined 相等
5. 存在 NaN 则返回 false


#### []==![] {}==!{}的结果
```js
console.warn([] == ![])//true
console.warn({} == !{})//false

// Number([]) //=> 0
// [].valueOf() -> []
// [].toString() -> ''
//  Number('') -> 0
// Number({})// => NaN
// ({}).valueOf() -> {}
// ({}).toString() -> '[object Object]'
// Number('[object Object]') -> NaN
```

### 深拷贝和浅拷贝

![深拷贝浅拷贝](book_files/24.jpg)

浅拷贝只复制对象的顶层属性和值，而深拷贝则会层层递归。

+ JavaScript中的`Object.assign()` `Object.create` 方法和数组和对象的`扩展运算符`（...）都执行的是浅拷贝。数组的concat和slice也属于浅拷贝
+ JSON.parse(JSON.stringify(obj))深拷贝,但是会忽略掉value是undefined,Symbol,function,也不能处理对象循环引用的问题,set类型数据和map类型会被处理为{}

```js
const fxArr = ["One", "Two", "Three"]
const fxArrs = fxArr.slice(0)
fxArrs[1] = "love";
console.log(fxArr) // ["One", "Two", "Three"]
console.log(fxArrs) // ["One", "love", "Three"]
```
```js
const fxArr = ["One", "Two", "Three"]
const fxArrs = fxArr.concat()
fxArrs[1] = "love";
console.log(fxArr) // ["One", "Two", "Three"]
console.log(fxArrs) // ["One", "love", "Three"]
```

### 数据类型
+ 基本类型： number string boolean undefined `null symbol` bigInt
+ 引用类型： object function array

Symbol 代表创建后独一无二且不可变的数据类型，它主要是为了解决可能出现的全局变量冲突的问题。

基本类型存储在栈中，引用类型存储在堆中
![栈](book_files/25.jpg)
![堆](book_files/26.jpg)

```js
let intNum = 55 // 10进制 55
let num1 = 070 // 8进制 56 (0开头)
let hexNum1 = 0xA //16进制 10(0x开头)
```

#### undefined和null的区别
+ undefined是声明后未赋值，而null通常表示一个空的对象引用(空指针)
+ 在早期的 JavaScript 引擎中，为了性能优化，`变量类型信息`被存储在一个变量的低位字节中。这些类型标签被用来快速判断变量的类型。由于当时的设计决策，null 和某些对象类型`共享`了相同的低位字节表示，这导致了 typeof null 返回 "object"。而undefined的typeof就是undefined
+ 在两等情况下，二者相等，三等情况下则不相等

#### 判断数据类型的方法
+ typeof可判断基础类型，函数类型和其他引用类型都返回object，注意null的特殊性
+ instanceOf可以用来测试构造函数的prototype属性是否出现在对象的`原型链`中的`任何位置`，它可以对引用类型准确判断，但是不能判断基础类型
+ Object.prototype.toString [object xxx]
+ 构造函数:存在风险，可以判断包括简单类型、引用类型的构造函数

```js
(2).constructor
//ƒ Number() { [native code] }
```

```js
function F1(){}
function F2(){}
F1.prototype = new F2()
// F1.prototype.constructor = F1
const f1 =new F1()

console.log(f1.constructor) //不注释的话为F1,注释的话为F2
console.log(f1 instanceof F1)//true
console.log(f1 instanceof F2)//true
```

### 数据结构
数据结构是计算机存储和组织数据的方式，它研究的是数据的`逻辑结构和物理结构`以及它们之间的相互关系

数据结构有多种分类，包括`数组、栈、链表、队列、树、图、字典、堆和散列表（哈希表）`等。

#### 数组和链表的应用场景

在前端开发中，链表和数组都是常用的数据结构，但它们有各自的特点和适用场景

1. 数组（Array）

特点：

+ 连续内存空间：数组的元素在内存中是`连续存储`的，这意味着访问数组中的任何元素都非常快（平均时间复杂度为`O(1)`）。
+ 固定大小：一旦数组被创建，其大小`通常是固定的`。虽然JavaScript中的数组是动态的，可以添加和删除元素，但内部实现可能会涉及到元素的移动和内存分配。
+ 随机访问：由于数组在内存中是连续存储的，所以可以通过索引直接访问任何元素。

适用场景：
+ 当需要`快速访问和修改特定位置`的元素时。
+ 当需要`保持元素的插入顺序`时。
+ 当处理大量静态数据或进行批量操作时。

2. 链表（Linked List）

特点：

+ 非连续内存空间：链表中的元素可以`分散`在内存中的任何位置，每个元素（节点）都包含数据和指向下一个元素的指针。
+ 动态大小：链表的大小可以`动态`地增长和缩小，因为节点可以根据需要添加或删除。
+ 顺序访问：访问链表中的元素通常需要从头节点开始，逐个遍历节点，直到找到所需的元素。这导致访问特定位置的元素的时间复杂度较高（平均时间复杂度为`O(n)`）。

适用场景：
+ 当需要在`任意位置插入或删除元素时`，链表的效率更高，因为不需要移动其他元素。
+ 当处理`大量动态数据`时，链表可以更有效地管理内存。
+ 当需要实现特定功能的数据结构（如栈、队列等）时，链表可以作为基础结构。

3. 对比
   + 空间复杂度：数组在内存使用上通常更紧凑，因为它们使用连续的内存空间。而链表由于每个节点都需要存储`指针`，可能会占用更多的空间。
   + 时间复杂度：访问数组中的特定元素通常更快，因为可以通过`索引`直接访问。而访问链表中的元素需要从头节点开始遍历，时间复杂度较高。但在`插入和删除操`作方面，链表通常比数组更高效。
   + 灵活性：链表在`动态调整大小和插入/删除元素`方面更加灵活，而数组则更适合处理`静态数据或进行批量操作`。

4. 总结
   
在选择使用数组还是链表时，需要根据具体的应用场景和需求来权衡。在大多数情况下，数组是前端开发中更常用的数据结构，因为它们提供了直观的访问方式和丰富的API。但在某些特定场景下，如需要频繁地插入或删除元素，或者实现特定的数据结构时，链表可能会是一个更好的选择。

### 字符串常用的方法
+ 增：concat(拼接，不影响原字符串)
+ 删：slice substr substring(都不影响原字符串)
+ 改：trim repeat padStart(填充) toLowerCase
+ 查：indexOf includes chartAt startWith
+ 转换方法： split(转为数组),可以是正则或者字符串
+ 正则方法：match search(可以是正则或者字符串,返回第一个匹配的内容下标) replace

```js
let colorText = "red,blue,green,yellow";
let colors1 = colorText.split(",");       // ["red", "blue", "green", "yellow"]
let colors2 = colorText.split(",", 2);    // ["red", "blue"]
let colors3 = colorText.split(/[e]+/);  // ['r', 'd,blu', ',gr', 'n,y', 'llow']
console.log(colors1)
console.log(colors2)
console.log(colors3)
```

```js
var str="Visit Runoob!"; 
var n=str.search("Runoob");
//6

let text = "cat,bat,sat,fat";
let pos = text.search(/at,f/);
console.log(pos); // 9
```

```js
var str="Visit Microsoft! Visit Microsoft!";
var n=str.replace("Microsoft","Runoob");
//"Visit Runoob! Visit Microsoft!"
var str2="Mr Blue has a blue house and a blue car";
var n2=str.replace(/blue/g,"red");
//"Mr Blue has a red house and a red car"
```

```js
//replace第二个参数还可以是函数
function htmlEscape(text) {
  return text.replace(/[<>"&]/g, function(match, pos, originalText) {
    switch(match) {
      case "<":
        return "&lt;";
      case ">":
        return "&gt;";
      case "&":
        return "&amp;";
      case "\"":
        return "&quot;";
    }       
  });
}
         
console.log(htmlEscape("<p class=\"greeting\">Hello world!</p>")); 
// "&lt;p class=&quot;greeting&quot;&gt;Hello world!&lt;/p&gt;"
```

#### match matchAll

match() 查找找到一个或多个正则表达式的匹配。
```js
var str="The rain in SPAIN stays mainly in the plain"; 
var n=str.match(/ain/g);
//ain,ain,ain
```

String.prototype.matchAll() 方法返回一个迭代器，它产生所有正则表达式匹配的结果，`包括捕获组`。
```js
const str = 'hello javascript hello css'
console.log(...str.matchAll(/(hel)(lo)/g))
//[ 'hello', 'hel', 'lo' ]  
//[ 'hello', 'hel', 'lo' ]
//浏览器打印会出现index和input等信息 ['hello', 'hel', 'lo', index: 0, input: 'hello javascript hello css', groups: undefined]
```
正则表达式 /(hel)(lo)/g 会查找字符串 str 中所有 hel 后面紧跟着 lo 的位置，并且因为它有两个捕获组 (hel) 和 (lo)，所以每个匹配项都会包含这两个捕获组。
```js
[  
  [完整匹配项, 捕获组1, 捕获组2, ...],  
  [完整匹配项, 捕获组1, 捕获组2, ...],  
  ...  
]
```
#### substring slice字符串处理区别
+ substring可以反向截取，既第二个参数可以比第一个参数小，而slcie不可以
+ slice支持负值，就是字符串末尾开始计算，而substring遇到负值自动归为0


### 数组常用的方法
+ 增 push unshift splice concat(只有它不影响原数组)
+ 删 pop shift splice slice(只有它不影响原数组)
+ 改 splice
+ 查 find(返回第一个匹配的选项) indexOf includes
+ 排序 sort reverse(会修改原数组)
+ 转换方法 join
+ 迭代方法 some every map forEach reduce filter 
+ 其他 fill flat(函数扁平化) copyWithin arr.keys() arr.values() arr.entries()

```js
let arr = [4,1,3,5,7,9]
let a=arr.find(e=>{
	return(e>5)
})
console.log(a)//7
```

#### 影响到原数组的方法
+ vue2中重写了 push pop unshift shift splice sort reverse
+ fill copyWithin
```js
let arr = [1, 2, 3, 4, 5];  
arr.fill('a', 1, 3); // arr 现在是 [1, 'a', 'a', 4, 5]
```
```js
let arr = [1,3,5,7,9]
arr.copyWithin(-2,0,2)
console.log(arr)//[1, 3, 5, 1, 3]
```

### event loop
js是单线程，同一时间只能做一件事，而避免阻塞的方法就是事件循环

+ 同步任务：都在主线程(这里的主线程就是JS引擎线程)上执行，会形成一个执行栈
+ 异步任务：事件触发线程管理着一个任务队列，只要异步任务有了运行结果，就在任务队列之中放一个事件回调

+ macro-task(宏任务)：包括整体代码script，setTimeout，setInterval，setImmediate ()-Node,requestAnimationFrame (存在争议,但是符合宏任务特征)-浏览器，I/O 操作、UI 渲染（浏览器）.
+ micro-task(微任务)：Promise，process.nextTick（Node.js）,Object.observe,MutationObserver,在ECMAScript中，microtask也被称为jobs

![同步异步](book_files/30.jpg)
![事件循环机制](book_files/29.jpg)
![宏任务与微任务](book_files/31.jpg)


#### event loop 宏任务 微任务 和dom渲染的关联
每一次 call stack（当前轮询结束） 结束，都会触发 DOM 渲染（**不一定非得渲染，就是给一次 DOM 渲染的机会！！！**）然后再进行 event loop。
+ 宏任务：DOM 渲染后再触发，ES 语法没有，JS 引擎不处理，浏览器（或 nodejs）干预处理。
+ 微任务：DOM 渲染前会触发，ES 语法标准之内，JS 引擎来统一处理。即，不用浏览器有任何关于，即可一次性处理完，更快更及时。

![处理](book_files/27.jpg)


```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title></title>
  </head>
  <script src="http://libs.baidu.com/jquery/2.0.0/jquery.min.js"></script>
  <body>
    <div id="container">
      
    </div>
  </body>
</html>
<script>
// 修改 DOM
const $p1 = $('<p>一段文字</p>')
const $p2 = $('<p>一段文字</p>')
const $p3 = $('<p>一段文字</p>')
$('#container')
    .append($p1)
    .append($p2)
    .append($p3)
console.log($('#container p'))
// 微任务：渲染之前执行（DOM 结构已更新，未渲染在页面上）
Promise.resolve().then(() => {
    const length = $('#container').children().length
    alert(`micro task ${length}`)
})

// 宏任务：渲染之后执行（DOM 结构已更新）
setTimeout(() => {
    const length = $('#container').children().length
    alert(`macro task ${length}`)
})
</script>
```

![微任务执行](book_files/28.jpg)


### Class和实例的关系以及原型链
1.	每个class都有显示原型prototype【class还有constructor和__proto__】
2.	每个实例都有隐式原型__proto__【实例无prototype，有constructor】
3.	实例的__proto__指向对应class的prototype

原型（prototype）主要指的是一个对象，它用于存储`共享属性和方法`。js每个函数都有一个prototype属性，这个属性是一个指针，指向一个对象，这个对象就是原型对象。可提高代码的复用性和效率。

原型链（prototype Chain）则是js实现继承的机制之一。每个对象都有一个`内部链接`指向另一个对象。这个链接被称为`对象的原型`。当试图访问一个对象的属性时，如果对象本身没有这个属性，那么js就会在这个对象的原型上寻找这个属性，这个原型对象自身也可能有原型，这样一层一层向上查找，就形成了一个链条，这就是原型链。

原型链不仅用于`实现继承`，还用于解决对象`属性查找`的问题。

![图1](book_files/33.jpg)
![图2](book_files/32.jpg)
![图3](book_files/34.jpg)

特殊之处：Function的构造函数是自己；Function的prototype和__proto__都指向`Function.prototype`，Function作为对象时，它的__proto__指向它构造函数的prototype。Object的构造函数也是Function,同时`一切对象又是Object创建`。

> 一切函数对象（包括Object），都是继承Function对象 

#### 函数的特殊性
+ Person作为一个函数对象，它的隐式原型指向`Function.prototype`
+ Person的prototype作为一个普通对象，它的隐式原型指向`Object.prototype`
+ Function的`隐式原型和显示原型`则相等
+ Object作为一个函数对象，它的隐式原型指向Function.prototype
  
```js
function Person(){}
const person = new Person()
console.log(Person.__proto__ === Function.prototype) // true
console.log(Person.prototype.__proto__ === Object.prototype)// true
console.log(Function.__proto__ === Function.prototype)// true
console.log(Function.prototype.__proto__ === Object.prototype)// true
console.log(Object.__proto__ === Function.prototype)// true
console.log(Object.prototype.__proto__ === null)// true
```

### js如何实现继承
它允许创建一个新的类（或对象），该类（或对象）可以继承另一个类（或对象）的属性和方法。

#### 继承的方案
+ 原型链继承：复用prototype方法，缺点：1.对象类型的属性会被其他实例修改2.打印无法直接看到继承的属性3.原型创建时无法传参
```js
// 无法定制化传参
Cat.prototype = new Animal();
Cat.prototype.constructor = Cat;
```

+ 盗用构造函数继承：子类可传参，不会共用对象，但是父类prototype方法无法继承
```js
function Child(){
	Parent.apply(this, [test]); 
}
```
+ 组合继承：结合原型链+构造函数，两者优点有，缺点：调用两次父类，浪费性能，且继承的属性子类已经通过盗用构造函数存在了，父类对象又存在了一份无用的


+ 原型式继承【用于字面量对象】,缺点：修改对象属性会改变其他对象的相关属性
```js
var obj = {
  name: "why",
  age: 18,
  arr:[]
}
var info = Object.create(obj)//浅拷贝
info.name =xxx 
info.arr.push(1)
```

+ 寄生式继承【用于字面量对象】，核心：`使用原型式继承获得一个目标对象的浅复制，然后增强这个浅复制的能力。`缺点同上
```js
function createAnother(original){
    var clone = Object.create(original);//通过调用函数创建一个新对象
	// 增加了某些功能
    clone.sayHi = function () {//以某种方式来增强这个对象
        console.log("Hi");
    };
    return clone; //返回这个对象
}
```

+ 寄生组合式继承：比较完美的方案
```js
function clone (parent, child) {
 // Object.create
 child.prototype = Object.create(parent.prototype);
 child.prototype.constructor = child;
}
function Parent6() {
 this.name = 'parent6';
 this.play = [1, 2, 3];
}
Parent6.prototype.getName = function () {
 return this.name;
}
function Child6() {
 Parent6.call(this);
 this.friends = 'child5';
}
clone(Parent6, Child6);
Child6.prototype.getFriends = function () {
 return this.friends;
}
let person6 = new Child6();
console.log(person6);
```

![整体思路](book_files/35.jpg)