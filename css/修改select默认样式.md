## 修改select的默认样式
> 我们需要在select外面包裹一层盒子，以便我们继续操作，通过伪类和定位来操作自己需要替换的图标
```less
    .option {
        margin-right: 20px;
        position: relative;

        select {
            position: relative;
            /*清除select的边框样式*/
            // border: none;
            /*清除select聚焦时候的边框颜色*/
            outline: none;

            /*隐藏select的下拉图标*/
            appearance: none;
            -webkit-appearance: none;
            -moz-appearance: none;
            /*通过padding-left的值让文字居中*/
            padding-left: 20px;

        }

        /*使用伪类给select添加自己想用的图标*/
        &:after {
            display: inline-block;
            content: " ";
            width: 11px;
            height: 11px;
            position: absolute;
            right: 0;//自己设置方位
            top: 0;

            background: url('图标') no-repeat center;
            background-size: 11px;
            /*给自定义的图标实现点击下来功能*/
            pointer-events: none;
        }
    }
```