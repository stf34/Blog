### 当前组件添加定时器，跳转其他页面自动清除
1. 解决方法1
> 此方法是全局使用定时器，然后在beforeDestroy() 生命周期里面将其销毁

```js

    beforeDestroy() {
        clearInterval(this.timer);
        this.timer = null;
    }
```

2. 解决方法2
> 该方法是通过$once这个事件侦听器器在定义完定时器之后的位置来清除定时器。以下是完整代码,此解决方法，代码解读性强些，简洁

``` js
    const timer = setInterval(() =>{
        // 某些定时器操作
    }, 500);
    // 通过$once来监听定时器，在beforeDestroy钩子可以被清除。
    this.$once('hook:beforeDestroy', () => {
        clearInterval(timer);
    })
```