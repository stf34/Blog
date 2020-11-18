### 单行文本超出出现省略号
```css
    overflow: hidden;/* 超出文本溢出隐藏 */
    text-overflow:ellipsis;/* 溢出用省略号显示 */
    white-space: nowrap;/* 溢出不换行 */
```

### 多行文本超出显示省略号
```css
    overflow:hidden; /* 超出文本溢出隐藏 */
    text-overflow:ellipsis;/* 溢出省略号显示 */
    display:-webkit-box; /* 将文本当成弹性盒子 */
    -webkit-box-orient:vertical;/* 使文本从上往下排布，（原理弹性盒子模式） */
    -webkit-line-clamp:2; /* 这里的数字可以控制几行超出出现省略号 */
```