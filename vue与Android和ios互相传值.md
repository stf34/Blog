# VUE和Android，ios互相传值
> Android向Vue传数据
```js
    android5.0之前的可以用下面的方式：

    webView.loadUrl("javascript:getDataFromNative('" + mParam + "')");

    android5.0之后的可以用下面的方式：

    webView.evaluateJavascript("javascript:getDataFromNative('" +mId +"')", new ValueCallback() {

    @Override

        public void onReceiveValue(String s) {

    //此方法可以得到回调值

            ZJToastUtil.showShort(s);

        }

    });

    以上两种方法要在webView加载完成后调用，如下在onPageFinished后调用才有效：

    webView.setWebViewClient(new WebViewClient() {      

                      @Override           

     public void onPageFinished(WebView view, String url) {      

              super.onPageFinished(view, url);                webView.loadUrl("javascript:getDataFromNative('" + mParam + "')");                webView.evaluateJavascript("javascript:getDataFromNative('" + mId + "')", new ValueCallback() {           

             @Override                  

      public void onReceiveValue(String s) {                        //此方法可以得到回调值                         
    }                });            }        });    }

```
> IOS端传数据
```js
    也要在网页加载完调用才生效

    NSString *toVueSting = @"vickylizy";

    NSString *jsStr = [NSString stringWithFormat:@"getDataFromNative('%@')",toVueSting];

    [self->_wkWebView evaluateJavaScript:jsStr completionHandler:^(id _Nullable d, NSError * _Nullable error) {

    NSLog(@"返回---%@",d);//回调值

    }];

```
> Vue端接收数据
``` js
    created() {

        //Vue的方法给原生调用，则需要把方法挂在Window下面

        window.getDataFromNative = this.getDataFromNative;

     },

     methods: {

        getDataFromNative(params) {

          //params: 原生调用Vue时传值（params）给Vue

          console.log("得到原生传值结果:" + params);

          // alert(params);

          //  var dic = {

          //     'name': "vicky"

          // };

          // return dic; //回调给原生，可写可不

        },

    }

``` 

## Vue向Android ios 端传数据
> vue端传值
```html
    <template>
        <div>
            <p>输入数字并提交:</p>
            <input v-model='value'  type="number">
            <button @click="myFunction()">提交给android,ios</button>
        </div>
    </template>
```
``` js
    methods: {
        myFunction(){
            const u = navigator.userAgent;
            const isiOS = !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/);
            let value = this.value//要传递的值
            if (isiOS) {
                alert('ios')
                window.webkit.messageHandlers.方法名.postMessage(value);
            } else {
                alert('android')
                /* 前提：Android设备中使用h5项目，这里只是说vue的使用方法，在浏览器内调用会直接报错 */
                window.android.方法名(value)
            }  
        },
    }

```
> Android端接收数据
``` js
    //Vue调用Android方法

    webView.addJavascriptInterface(this, "$App");//添加js监听 这样html就能调用客户端

    @JavascriptInterface

    public void onItemClick(String msg) {

        Intent intent = new Intent(this, ProjectDetailActivity.class);

        intent.putExtra(ProjectDetailActivity.PROJECT_NAME, msg);

        startActivity(intent);

    }

```
> IOS端接收数据
``` js
    #pragma mark -WKScriptMessageHandler

    - (void)userContentController:(WKUserContentController*)userContentController didReceiveScriptMessage:(nonnull WKScriptMessage *)message{

    if ([message.name isEqualToString:@"onItemClick"]) {

    NSLog(@"是什么？---%@",message.body);

    //做原生操作

    }

    }
```

