## 基于silder.js的步骤条封装
 
最近在做组件开发相关的工作，所以就自己动手封装了一些自己的组件库，下面这个是封装的步骤条组件，是基于silder.js上进行开发封装的，主要应用：jQuery，silder.js这些逻辑
**主要：**是基于silder来封装的

### 原有silder.js

1. 属性：
    * elem:步骤条展示的`dome`组件
    * color: 颜色，
    * pos: 初始滑动百分比，
    * count: 最大值，
    * disable: 是否禁用组件
    * showNum: 是否显示进度
  
2. 事件
   * callBackMove: 鼠标滑动事件
    ```js
        callBackMove: function (num) {//鼠标滑动时事件
            console.log('move', num);
            
        },
    ```
   * callBackMouseup: 鼠标停止事件
    ```js
        callBackMouseup: function (num) {//鼠标停止事件
            console.log('up', num);
        }
    ```
### 基于原有silder.js 进行封装后，主要添加了`samall`属性

1. 新增属性
   * samall: 可以输入最小值
  
> 实现最小值和最大值是随时可控的
[封装组件silder.js路径]()

### 使用方法
```html
    <div class="a"></div>
```

> 前提：引入`<link rel="stylesheet" href="./css/silder.css">`样式
```css
    <style>
        .a {
            width: 500px;
            height: 20px;
            margin-top: 100px;
            margin-left: 100px;
        }
        /* 线宽 */
        .zui-slider-bar{
            height: 10px;
        }
        /* 圈的大小 */
        .zui-slider-wrap{
            top: -12px;
        }
        .zui-slider-wrap-btn{
            width: 16px;
            height: 16px;
        }
        html,
        body {
            height: 100%;
            overflow: hidden;
        }

        * {
            padding: 0;
            margin: 0;
        }
    </style>
```
> 前提：引入`<script src="./js/silder.js"></script>`代码逻辑
```js
    ZUI.silder({
        elem: '.a',
        color: ' #2fa0ec',//步骤条颜色
        pos: '35%',//初始滑动百分比
        samall:16,//最小值
        showNum: true,//是否显示当前字段
        count: 100,//最大值
        disable: false,//是否禁用
        callBackMove: function (num) {//鼠标滑动时事件
            console.log('move', num);
            
        },
        callBackMouseup: function (num) {//鼠标停止事件
            console.log('up', num);
        }
    }) 
```