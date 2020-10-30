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