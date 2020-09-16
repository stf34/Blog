
# vue 项目架构设计

## 1.初始搭建项目环境  
- 借鉴文章：
* vue移动端项目架构设计：  https://www.jianshu.com/p/d4598baf31c2
* router 路由拆分模块化：https://www.jianshu.com/p/2833243987dd
* vue中Axios的封装与API接口的管理：https://juejin.im/post/6844904038085967880
* vue-cli4环境变量与分环境打包方法：https://juejin.im/post/6844904038459244558
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
> UI框架，（这里我使用vant） 移动端：vant PC端：element UI 
