## 监听页面滚动，顶部导航栏固定定位
>methods里面的代码
``` vue
       /* 滚动监听定位 */
        promScroll () {
            var scrollTop = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop
            /* 下面的类目是你要监听的元素类名 */
            var offsetTop = document.querySelector('类名').offsetTop
            if (scrollTop > offsetTop) {
                这里可以写你要改变的事件或者是状态
            } else {
                这里可以写你要改变的事件或者是状态
            }
        },
```

### vue 点击元素滚动到指定位置
``` html
<template>
        <div class="div">
            <button ></button>
            <div id="Dom">
                .....内容
            </div>
        </div>
</template>
```
> 先在 mounted里面监听页面滑动
``` vue
    mounted() {
        // 监听页面滑动
        window.addEventListener('scroll', this.Scroll, true)
    },
```

> 然后在 methods里面写监听事件和方法
``` vue
    methods: {
        /* 滚动监听定位 */
        Scroll () {
            var scrollTop = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop
            /* 下面的类目是你要监听的元素类名 */
            // var offsetTop = document.querySelector('类名').offsetTop
            if (scrollTop > 400) {
                // 这里可以写你要改变的事件或者是状态
                // console.log('大于400');
            } else {
                // 这里可以写你要改变的事件或者是状态
                // console.log('小于400');
            }
        },
    }
```
> 最后在 离开当前页面时，销毁监听事件 不然会报错
``` vue
    destroyed () {
        /* 离开清除监听事件 */
        window.removeEventListener('scroll', this.Scroll)
    },
```