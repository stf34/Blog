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