


# 前端整理合集
- [前端整理合集](#前端整理合集)
  - [html](#html)
    - [如何理解html语义化](#如何理解html语义化)
    - [p标签里面不能嵌套ul、div等块级元素原因](#p标签里面不能嵌套uldiv等块级元素原因)
  - [css](#css)
    - [offsetWidth](#offsetwidth)
  - [js](#js)
    - [window.getComputedStyle(element)](#windowgetcomputedstyleelement)


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

### 清除浮动

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