### jS实现点击复制功能
``` html
    <button onClick="copyCode()">复制</button>
```
> 点击按钮，创建input，填充要复制的内容，然后剪切到鼠标右键剪切板上

```js
    /* 复制链接 */
    /* content：复制的内容, message：提示的消息 */
    function copyCode()(content, message) {

        var url = content//复制的网址或内容
        var aux = document.createElement("input");
        aux.setAttribute("value", url);
        document.body.appendChild(aux);
        aux.select();
        document.execCommand("Copy"); //原生copy方法执行浏览器复制命令
        document.body.removeChild(aux); //清除input
        if (message == null) {
            /* 复制失败消息 */
        } else {
            /* 复制成功消息 */
        }
    }
```