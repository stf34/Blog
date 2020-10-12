
# vue 项目架构设计

## 1.初始搭建项目环境  
- 借鉴文章：
    * vue移动端项目架构设计：  https://www.jianshu.com/p/d4598baf31c2
    * router 路由拆分模块化：https://www.jianshu.com/p/2833243987dd
    * vue中Axios的封装与API接口的管理：https://juejin.im/post/6844904038085967880
    * vue-cli4环境变量与分环境打包方法：https://juejin.im/post/6844904038459244558
    * 环境变量设置：https://blog.csdn.net/weixin_43891448/article/details/107370738 https://www.jianshu.com/p/e40709390301
    * vuex拆分模块化：https://www.jianshu.com/p/d40561bce5d0
-------
### 1.1 项目初始化 
> 技术选型：
1. 脚手架工具：`vue-cli 4`
    - 前端框架——vue
    - vue 状态管理——vuex
    - vue 路由管理——vue-router
    - 请求方式——axios
    - 样式管理——less
    - 包管理——npm/cnpm
### 1.2 vue-cli4 搭建项目
> vue 使用`vue-cli` ，不过脚手架更新很快，所以这里我们使用脚手架4
1. 安装 vue cli
    ```
        npm install -g @vue/cli
        # OR
        yarn global add @vue/cli

    ```
2. 如果安装过 `vue cli2` 的话,可以使用以下命令更新脚手架
    ```
        npm uninstall vue-cli -g
    ```
3. 查看是否安装成功
    ```
        vue --version
        #OR
        vue -V
    ```
4. 如果安装失败的话，可以先卸载 `vue cli`
    ```
        npm uninstall vue-cli -g 
        #OR
        yarn global remove vue-cli 卸载它。
    ```
-------
## 2 创建项目
    ```
        vue create vue-test
        //vue-test: 这是你创建的项目名字
    ```
> 空格选择 要使用的配置，回车确定配置

**步骤：**
<img src="./img/vue cli/vue-cli01.png"/>
<img src="./img/vue cli/vue-cli02.png"/>
<img src="./img/vue cli/vue-cli03.png"/>
<img src="./img/vue cli/vue-cli04.png"/>
<img src="./img/vue cli/vue-cli05.png"/>
<img src="./img/vue cli/vue-cli06.png"/>
<img src="./img/vue cli/vue-cli07.png"/>
<img src="./img/vue cli/vue-cli08.png"/>
<img src="./img/vue cli/vue-cli09.png"/>
<img src="./img/vue cli/vue-cli10.png"/>

### vue中Axios的封装与API接口的管理
> 安装 Axios 
    ```
        npm install axios 
    ```
1. 配置 Axios环境
    * 引入`axios`
        ```js
        import axios from 'axios';
        import QS from 'qs';
        import { Toast } from 'vant';//你要所引入框架的提示
        import store from '../store/index'//状态管理工具
        ```
    * 配置请求头和请求时间
        ```js
        // 请求超时时间
        axios.defaults.timeout = 5000;

        // post请求头
        axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded;charset=UTF-8';
        ```
    * 配置请求拦截器
        ```js
            // 请求拦截器
            axios.interceptors.request.use(
                config => {
                    // 判断是否存在token，如果存在，则统一在http请求的header都加上token
                    const token = store.state.token;
                    token && (config.headers.Authorization = token);
                    return config;
                },
                error => {
                    return Promise.error(error);
                }
            )
        ```
    * 配置响应拦截器
        ```js
            // 响应拦截器
            axios.interceptors.response.use(
                response => {
                    if (response.status === 200) {
                        return Promise.resolve(response);
                    } else {
                        return Promise.reject(response);
                    }
                },
                // 服务器状态码不是200的情况 
                error => {
                    if (error.response.status) {
                        switch (error.response.status) {
                            // 401: 未登录    
                            // 未登录则跳转登录页面，并携带当前页面的路径    
                            // 在登录成功后返回当前页面，这一步需要在登录页操作。    
                            case 401:
                                router.replace({
                                    path: '/login',
                                    query: { redirect: router.currentRoute.fullPath }
                                });
                                break;
                            // 403 token过期    
                            // 登录过期对用户进行提示    
                            // 清除本地token和清空vuex中token对象    
                            // 跳转登录页面    
                            case 403:
                                Toast('登录过期，请重新登录');
                                // 清除token     
                                localStorage.removeItem('token');
                                store.commit('loginSuccess', null);     // 不太懂的话可不对状态码进行操作
                                // 跳转登录页面，并将要浏览的页面fullPath传过去，登录成功后跳转需要访问的页面
                                setTimeout(() => {
                                    router.replace({
                                        path: '/login',
                                        query: {
                                            redirect: router.currentRoute.fullPath
                                        }
                                    });
                                }, 1000);
                                break;
                            // 404请求不存在    
                            case 404:
                                Toast('网络请求不存在');
                                break;
                            // 其他错误，直接抛出错误提示    
                            default:
                                Toast(error.response.data.message);
                        }
                        return Promise.reject(error.response);
                    }
                }
            );
        ```
    * 封装get请求方法
        ```js
            /** 
            * get方法，对应get请求 
            * @param {String} url [请求的url地址] 
            * @param {Object} params [请求时携带的参数] 
            */
            export function get (url, params) {
                return new Promise((resolve, reject) => {
                    axios.get(url, {
                        params: params
                    })
                        .then(res => {
                            resolve(res.data);
                        })
                        .catch(err => {
                            reject(err.data)
                        })
                });
            }
        ```
     * 封装post请求方法
        ```js
            /** 
            * post方法，对应post请求 
            * @param {String} url [请求的url地址] 
            * @param {Object} params [请求时携带的参数] 
            */
            export function post (url, params) {
                return new Promise((resolve, reject) => {
                    axios.post(url, QS.stringify(params))
                        .then(res => {
                            resolve(res.data);
                        })
                        .catch(err => {
                            reject(err.data)
                        })
                });
            }
        ```
### vue 中 router 路由拆分模块化
1. 首先在 `router` 文内建文件夹 `models`,里面新建单个路由模块组件，这里以首页路由`layout.js`为例
    ```js
        export default [
            {
            path: '/',
            icon: 'el-icon-lx-home',
            component: () => import('../../views/layout/home/main.vue'),
            meta: { title: '系统首页', name: ['系统首页'] }
            },
        ]
    ```
2. 创建完成之后，在`router`文件内的主文件上引入即可
    ```js
        import { createRouter, createWebHistory } from 'vue-router'
        import layout from './models/layout'//主页


        const routes = [
        ...layout,//主页路由

        ]

        const router = createRouter({
        history: createWebHistory(process.env.BASE_URL),
        routes
        })
        
        export default router
    ```
3. 然后启动项目就行`npm run serve`

> UI框架，（这里我使用vant） 移动端：vant PC端：element UI 
### 如果使用响应式布局

> PC端  rem + 媒体查询
1. css 样式
    ``` css
        @charset "utf-8";
        /* 媒体查询，响应式布局 */
        @media (min-width:1024px){
            body{font-size:18px;}
        }
        @media (min-width:1100px){
            body{font-size:20px;}
        }
        @media (min-width:1280px){
            body{font-size:22px;}
        }
        @media (min-width:1366px){
            body{font-size:24px;}
        }
        @media (min-width:1440px){
            body{font-size:25px;}
        }
        @media (min-width:1680px){
            body{font-size:28px;}
        }
        @media (min-width:1920px){
            body{font-size:33px;}
        }

        html, body, div, span, applet, object, iframe,
        h1, h2, h3, h4, h5, h6, p, blockquote, pre,
        a, abbr, acronym, address, big, cite, code,
        del, dfn, em, font, img, ins, kbd, q, s, samp,
        small, strike, strong, sub, sup, tt, var,
        b, u, i, center,
        dl, dt, dd, ol, ul, li,
        fieldset, form, label, legend,
        table, caption, tbody, tfoot, thead, tr, th, td {
            margin: 0;
            padding: 0;
            border: 0;
            outline: 0;
            font-size: 100%;
            /*background: transparent;*/
        }
        table {
            border-collapse:collapse;
            border-spacing:0;
        }
        fieldset, img {    border:0;}
        address, caption, cite, code, dfn, em, strong, th, var {
            font-style:normal;
            font-weight:normal;
        }
        ol, ul { list-style:none; }
        caption, th { text-align:left; }
        h1, h2, h3, h4, h5, h6 {
            font-size:200%;
            font-weight:normal;
        }
        :focus { outline: 0;}
        a{ text-decoration:none;}
        a:hover img{ border:none;}

        a:active{}
        /*清除浮动*/
        .clearfix:after {
            content: ".";
            display: block;
            height: 0;
            clear: both;
            visibility: hidden;
        }
        .clearfix {display: inline-block;}
        /* Hides from IE-mac \*/
        * html .clearfix { height: 1%;}
        .clearfix {display: block;}
        /* End hide from IE-mac */

    ```
2. js 逻辑
    ```js
        var resizeType = true
        
        /* 初始加载按照当前窗口自动渲染当前页内容 */
        Resize()
        /* 跟随窗口的改变而改变 */
        window.onresize = function () {
        if (resizeType) {
            Resize()
        }
        }
        function Resize(){
        var whdef = 100/1920;// 表示1920的设计图,使用100PX的默认值,其他页面其他尺寸、、手机表示750的设计图,使用50PX的默认值
        var wH = window.innerHeight;// 当前窗口的高度
        var wW = window.innerWidth;// 当前窗口的宽度
        if (wW < 1024) {
            resizeType = false
            document.documentElement.style.fontSize = 1+'px'
        } else {
            resizeType = true
        }
        var rem = wW * whdef;// 以默认比例值乘以当前窗口宽度,得到该宽度下的相应FONT-SIZE值
        document.documentElement.style.fontSize = rem+'px'
        }
    ```
> [移动端响应式布局](./vue基础配置.md)
