### video上传本地视频
``` html
    <!-- 视频展示区 -->
    <video width="400" height="300" src="" id="video0" preload="auto"
        autoplay="autoplay" loop="loop" webkit-playsinline controls="controls">
    </video>

    <!-- 上传按钮 -->
    <input class="form-control" type="file" style="height:auto;" id="video" name="video" />
    <button id="open">点击获取当前时间</button>
```
> viedo 属性介绍

* autopla：当视频准备就绪时，就会马上播放
* controls: 显示播放按钮，方便用户操作
* loop: 实现重复播放视频，播放完之后从新播放
* preload: 如果出现该属性，则视频在页面加载时进行加载，并预备播放。如果使用 "autoplay"，则忽略该属性
* src: 要播放的视频的 URL
* muted:	规定视频的音频输出应该被静音
* poster	URL	规定视频下载时显示的图像，或者在用户点击播放按钮前显示的图像

> 前提：需引入jQuery
```js
    // hTML5实现表单内的上传文件框，上传前预览视频，刷新预览video，使用HTML5 的File API,
    // 建立一个可存取到该file的url，一个空的video标签，ID为video0,把选择的文件显示在video标签中，实现视频预览功能。
    // 需要选择支持HTML API的浏览器。
    $("#video").change(function () {
        var objUrl = getObjectURL(this.files[0]);
        console.log("objUrl = " + objUrl);
        if (objUrl) {
            $("#video0").attr("src", objUrl);
        }
    });
    //建立一个可存取到该file的url
    function getObjectURL(file) {
        var url = null;
        if (window.createObjectURL != undefined) { // basic
            url = window.createObjectURL(file);
        } else if (window.URL != undefined) { // mozilla(firefox)
            url = window.URL.createObjectURL(file);
        } else if (window.webkitURL != undefined) { // webkit or chrome
            url = window.webkitURL.createObjectURL(file);
        }
        return url;
    }
    $('#open').click(function () {
        console.log(1);
        var yourtime = '2020-9-10';

        yourtime = yourtime.replace(/-/g, "/");//替换字符，变成标准格式
        var d2 = new Date();//取今天的日期
        var d1 = new Date(Date.parse(yourtime));
        //alert(d1);
        //alert(d2);
        if (d1 > d2) {
            alert("开始大于结束");

        } else {
            alert('开始时间小于结束时间')
        }
    })
```


## 遭遇的问题

1. 移动端网页 video播放层级过高遮挡自定义元素
> 解决方法：(在video上添加属性)
    ios :`webkit-playsinline="true"`
    android:`x5-video-player-type="h5" x5-video-player-fullscreen="true" x5-video-orientation="portraint"`

2. video标签限制宽和高
```css
    video{
        object-fit:fill;
    }
```


fill: 默认值。替换内容拉伸填满整个content box, 不保证保持原有的比例。
contain: 保持原有尺寸比例。保证替换内容尺寸一定可以在容器里面放得下。因此，此参数可能会在容器内留下空白。
cover: 保持原有尺寸比例。保证替换内容尺寸一定大于容器尺寸，宽度和高度至少有一个和容器一致。因此，此参数可能会让替换内容（如图片）部分区域不可见。
none: 保持原有尺寸比例。同时保持替换内容原始尺寸大小。
scale-down: 就好像依次设置了none或contain, 最终呈现的是尺寸比较小的那个。