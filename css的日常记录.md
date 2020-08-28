### 关于height:100%和height:100vh的区别
* vh就是当前屏幕可见高度的1%，也就是说

* height:100vh == height:100%;

* 但是当元素没有内容时候，设置height:100%，该元素不会被撑开，此时高度为0，

* 但是设置height:100vh，该元素会被撑开屏幕高度一致



### CSS实现垂直居中的常用方法
> 如果是水平居中的话，直接设置盒子的宽和高，然后直接 margin：0 auto;就行
* 垂直居中的话，我们可以使用定位来实现，和方位名词，top，bottom , right,left 
> 由于position的值为static（静止的、不可以移动的），元素在文档流里是从上往下、从左到右紧密的布局的，我们不可以直接通过top、left等属性改变它的偏移。所以，想要移动元素的位置，就要把position设置为不是static的其他值，如relative,absolute,fixed等。然后，就可以通过top、bottom、right、left等属性使它在文档中发生位置偏移（注意，relative是不会使元素脱离文档流的，absolute和fixed则会！也就是说，relative会占据着移动之前的位置，但是absolute和fixed就不会）。设置了position: relative后的代码如下
``` html
<template>
    <div class="bgPage">
        <div class="Content">

        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {

        };
    },
    created() {

    },
    mounted() {

    },
    methods: {
    }
}
</script>

<style scoped lang="less">
.bgPage{
    width: 100%;
    height: 100vh;
    background: linear-gradient(45deg,rgba(254,172,94,0.5),rgba(199,121,208,0.5),rgba(75,192,200,0.5));
    .Content{
        display: flex;
        justify-content: center;
        justify-items: center;
        flex-wrap: wrap;
        width: 400px;
        height: 400px;
        margin: 0 auto; /*水平居中*/
        position: relative;
        top: 50%; /*偏移*/
    }
}
</style>

```
* div垂直方向上面并没有居中,因为我们往下垂直定位时，并没有减去盒子的一半，所以，我们使用以下代码
``` html
<template>
    <div class="bgPage">
        <div class="Content">

        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {

        };
    },
    created() {

    },
    mounted() {

    },
    methods: {
    }
}
</script>

<style scoped lang="less">
.bgPage{
    width: 100%;
    height: 100vh;
    background: linear-gradient(45deg,rgba(254,172,94,0.5),rgba(199,121,208,0.5),rgba(75,192,200,0.5));
    .Content{
        display: flex;
        justify-content: center;
        justify-items: center;
        flex-wrap: wrap;
        width: 400px;
        height: 400px;
        margin: 0 auto; /*水平居中*/
        position: relative;
        top: 50%; /*偏移*/
        transform: translateY(-50%);
        /* 或者 */
        margin-top: -150px; 
    }
}
</style>

```