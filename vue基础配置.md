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
