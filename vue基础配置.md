## vue自动打开浏览器配置
### vue 配置

> 找到config/index.js中
* 1.将autoOpenBrowser: false 修改为 autoOpenBrowser: true
* 2.然后重新启动服务就行
### vue 脚手架3 以上版本 配置
> 在根目录下找到 package.json

* 1.找到配置项‘scripts’

* 2.找到配置项‘serve’，添加 "serve": "vue-cli-service serve --open",
* 3.重新启动就可以 自动打开浏览器了



## vue 移动端配置rem 响应式布局
### vue-cli2.0移动端适配 
> 安装：npm install px2rem-loader --save  
* 它的作用是将你写好的px转换成rem，这样你就可以正常的按设计稿去写px，但是实际到页面上就成了rem
!> 然后在目录  build/utils.js 内下这样配置
``` vue
    exports.cssLoaders = function (options) {
    options = options || {}

    const cssLoader = {
        loader: 'css-loader',
        options: {
        sourceMap: options.sourceMap
        }
    }

    const postcssLoader = {
        loader: 'postcss-loader',
        options: {
        sourceMap: options.sourceMap
        }
    }
    /*
        这里是需要引入的px2rem-loader, 重要步骤
    */
    var px2remLoader = {
        loader: 'px2rem-loader',
        options: {
            remUnit: 7.5//设计稿宽度/10
        }
        };
    // generate loader string to be used with extract text plugin
    function generateLoaders (loader, loaderOptions) {
        // const loaders = options.usePostCSS ? [cssLoader, postcssLoader] : [cssLoader]
        var loaders = [cssLoader, px2remLoader];//添加px2rem 插件
        if (loader) {
        loaders.push({
            loader: loader + '-loader',
            options: Object.assign({}, loaderOptions, {
            sourceMap: options.sourceMap
            })
        })
        }

        // Extract CSS when that option is specified
        // (which is the case during production build)
        if (options.extract) {
        return ExtractTextPlugin.extract({
            use: loaders,
            fallback: 'vue-style-loader',
            publicPath: '../../'
        })
        } else {
        return ['vue-style-loader'].concat(loaders)
        }
    }

    // https://vue-loader.vuejs.org/en/configurations/extract-css.html
    return {
        css: generateLoaders(),
        postcss: generateLoaders(),
        less: generateLoaders('less'),
        sass: generateLoaders('sass', { indentedSyntax: true }),
        scss: generateLoaders('sass'),
        stylus: generateLoaders('stylus'),
        styl: generateLoaders('stylus')
    }
    }
```
### 然后在index.html中加入适配代码就可以了
``` html
    <script>
        fnResize()
        window.onresize = function () {
        fnResize()
        }
        function fnResize() {
        var deviceWidth = document.documentElement.clientWidth || window.innerWidth;
        document.documentElement.style.fontSize = (deviceWidth / 70) + 'px';//这个70可以根据实际情况修改
        }
    </script>
```
### vue-cli3.0移动端适配 
> 如果你使用的是css 安装： npm install px2rem-loader --save
* 1.安装完之后，需要在根目录，也就是 src同级的文件 新建 vue.config.js 文件
* 2.然后在此文件内引入 刚下载的px2rem-loader
``` vue
    module.exports = {
        chainWebpack: (config) => {
            config.module
            .rule('css')
            .test(/\.css$/)
            .oneOf('vue')
            .resourceQuery(/\?vue/)
            .use('px2rem')
            .loader('px2rem-loader')
            .options({
                /*
                这里是换算的设计稿的宽度
                设计稿是750宽的话就是7.5
                */
                remUnit: 7.5 
            })
        }
    }
```
> 因为是要换算px 为rem 所以我们可以在index.html文件内加入以下代码，然后重启项目就行了
``` html
    <!-- head中加入 -->
    <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no">

    <script>
        fnResize()
        window.onresize = function () {
            fnResize()
        }
        function fnResize() {
            var deviceWidth = document.documentElement.clientWidth || window.innerWidth;
            /*
            deviceWidth / 75   这里的75可以根据实际情况去修改
            */
            document.documentElement.style.fontSize = (deviceWidth / 75) + 'px';
        }
    </script>
```
> 如果项目中使用的是less，scss这种预编译语言的话，就需要使用 postcss-plugin-px2rem这个插件了
> 安装：npm install postcss-plugin-px2rem --save
* 1.同样在 vue.config.js 文件内配置
``` vue
    module.exports = {
        css: {
            loaderOptions: {
                postcss: {
                    plugins: [
                        require('postcss-plugin-px2rem')({
                            rootValue: 7.5, //换算基数， 默认100  ，这样的话把根标签的字体规定为1rem为50px,这样就可以从设计稿上量出多少个px直接在代码中写多上px了。
                            // unitPrecision: 5, //允许REM单位增长到的十进制数字。
                            //propWhiteList: [],  //默认值是一个空数组，这意味着禁用白名单并启用所有属性。
                            // propBlackList: [], //黑名单
                            exclude: false, //默认false，可以（reg）利用正则表达式排除某些文件夹的方法，例如/(node_module)\/如果想把前端UI框架内的px也转换成rem，请把此属性设为默认值
                            // selectorBlackList: [], //要忽略并保留为px的选择器
                            // ignoreIdentifier: false,  //（boolean/string）忽略单个属性的方法，启用ignoreidentifier后，replace将自动设置为true。
                            // replace: true, // （布尔值）替换包含REM的规则，而不是添加回退。
                            mediaQuery: false, //（布尔值）允许在媒体查询中转换px。
                            minPixelValue: 3 //设置要替换的最小像素值(3px会被转rem)。 默认 0
                        }),
                    ]
                }
            }
        }
    }
```
* 在index.html 内添加 以下代码
``` html
    <script>
        fnResize()
        window.onresize = function () {
        fnResize()
        }
        function fnResize() {
            var deviceWidth = document.documentElement.clientWidth || window.innerWidth;
            /*
            deviceWidth / 75   这里的75可以根据实际情况去修改
            */
            document.documentElement.style.fontSize = (deviceWidth / 75) + 'px';
        }
    </script>
```