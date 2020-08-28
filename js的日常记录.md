### 获取浏览器窗口的可视区域高度和宽度
> js获取 浏览器的高和宽
``` html
基本逻辑如代码，关键在于获取屏幕可见高度的兼容性document.documentElement.clientHeight之前是这样获取的，PC端没什么问题，就是手机浏览器打开时，出现异常。网上查了下，最终的实现代码

var h = window.innerHeight || document.documentElement.clientHeight || document.body.clientHeight;

document.body.scrollTop || document.documentElement.scrollTop
 1 document.body.clientWidth ==> BODY对象宽度
 2 document.body.clientHeight ==> BODY对象高度
 3 document.documentElement.clientWidth ==> 可见区域宽度
 4 document.documentElement.clientHeight ==> 可见区域高度
 5   
 6 document.body.clientWidth ==> 网页可见区域宽 
 7 document.body.clientHeight ==> 网页可见区域高
 8 document.body.offsetWidth ==> 网页可见区域宽(包括边线的宽)
 9 document.body.offsetHeight ==> 网页可见区域高(包括边线的高)
10 document.body.scrollWidth ==> 网页正文全文宽document.body.scrollHeight ==> 网页正文全文高
11 document.body.scrollTop ==> 网页被卷去的高
12 document.body.scrollLeft ==> 网页被卷去的左
13 window.screenTop ==> 网页正文部分上
14 window.screenLeft ==> 网页正文部分左
15 window.screen.height ==> 屏幕分辨率的高
16 window.screen.width ==> 屏幕可用工作区高度
17 window.screen.availHeight ==> 屏幕可用工作区高度
18 window.screen.availWidth ==> 屏幕可用工作区宽度
```
> jQuery获取 浏览器的高和宽
``` html
1 // 部分jQuery函数  
2 $(window).height() 　              //浏览器时下窗口可视区域高度   
3 $(document).height()　           //浏览器时下窗口文档的高度   
4 $(document.body).height()　　　　　　//浏览器时下窗口文档body的高度   
5 $(document.body).outerHeight(true)　//浏览器时下窗口文档body的总高度 包括border padding margin   
6 $(window).width() 　   //浏览器时下窗口可视区域宽度   
7 $(document).width()   //浏览器时下窗口文档对于象宽度   
8 $(document.body).width()　　　　　　//浏览器时下窗口文档body的高度   
9 $(document.body).outerWidth(true)　//浏览器时下窗口文档body的总宽度 包括border padding  
```
``` html
 1 HTML精确定位:  scrollLeft,scrollWidth,clientWidth,offsetWidth   
 2 scrollHeight: 获取对象的滚动高度。   
 3 scrollLeft: 设置或获取位于对象左边界和窗口中目前可见内容的最左端之间的距离   
 4 scrollTop:  设置或获取位于对象最顶端和窗口中可见内容的最顶端之间的距离   
 5 scrollWidth: 获取对象的滚动宽度   
 6 offsetHeight:获取对象相对于版面或由父坐标 offsetParent 属性指定的父坐标的高度   
 7 offsetLeft: 获取对象相对于版面或由 offsetParent 属性指定的父坐标的计算左侧位置   
 8 offsetTop:  获取对象相对于版面或由 offsetTop 属性指定的父坐标的计算顶端位置   
 9 event.clientX 相对文档的水平座标   
10 event.clientY 相对文档的垂直座标   
11 event.offsetX 相对容器的水平坐标   
12 event.offsetY 相对容器的垂直坐标   
13 document.documentElement.scrollTop 垂直方向滚动的值   
14 event.clientX+document.documentElement.scrollTop 相对文档的水平座标+垂直方向滚动的量 

alert($(document).scrollTop()); //获取滚动条到顶部的垂直高度
alert($(document).scrollLeft()); //获取滚动条到左边的垂直宽度
```