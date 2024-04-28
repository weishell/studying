


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
    - [rem em vw vh](#rem-em-vw-vh)
    - [flex布局](#flex布局)
  - [js](#js)
    - [window.getComputedStyle(element) 获取伪类中的内容](#windowgetcomputedstyleelement-获取伪类中的内容)


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

### rem em vw vh
1.	rem: 相对大小，但相对的只是HTML根元素
2.	em:  继承父级元素的字体大小
3.	vw:window.innerWidth = 100vw
4.	vh:window.innerHeight = 100vh
5.	vmax:取vh/vw中大值
6.	vmin: 取vh/vw中小


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