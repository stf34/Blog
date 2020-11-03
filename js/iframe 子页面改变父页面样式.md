###

> 父元素改子元素样式（不跨域）：
```js
    document.getElementById('iframeID').contentWindow.document.body.style.backgroundColor="#f00";
```

> 子元素改变父元素样式
```js
    var topWindow = $(window.parent.document);

    var $iframe = topWindow.find('标签名/类名');//查找相应的类名iframe
    
    $iframe.contents().find('input.sunPageSearch').siblings('ul').css('display', 'none');
    $iframe.css('padding', '0');
```