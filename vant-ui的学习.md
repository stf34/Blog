 ## vant UI 的基本配置
> 通过 npm 安装, 在现有项目中使用 Vant 时，可以通过npm或yarn安装
### 通过 npm 安装
> npm i vant -S

### 通过 yarn 安装
> yarn add vant

## 使用 Collapse 折叠面板 搭建移动端左侧菜单栏项
> 你可以在根目录的 assets文件夹内新建一个 js 文件 去，内建一个vant.js 的文件，配置你要 按需引入的组件
* 1.前提是：你如果想要自动配置的话，你可以使用官方提供的一个自动按需引入的插件：babel-plugin-import 是一款 babel 插件，它会在编译过程中将 import 的写法自动转换为按需引入的方式
> npm安装： npm i babel-plugin-import -D  可以在 babel.config.js 中配置 

``` vue
    module.exports = {
        plugins: [
            ['import', {
            libraryName: 'vant',
            libraryDirectory: 'es',
            style: true
            }, 'vant']
        ]
    };
``` 
> asssts文件夹内的 vant.js 写法
``` vue
    import Vue from 'vue'
    import { 
        Button,
        Collapse,
        CollapseItem
    } from 'vant'
    Vue
    .use(Button)
    .use(Collapse)
    .use(CollapseItem)//Collapse 折叠面板
```
> 在main.js 内引入 
``` vue
    import './assets/js/vant'//引入vant UI 相关组件配置
```

> 在组件中使用
``` html
    <template>
        <div class="sidernav">
            <!-- 通过accordion可以设置为手风琴模式，最多展开一个面板，此时activeName为字符串格式 -->
            <van-collapse v-model="activeNames" accordion>
                <van-collapse-item name="1">
                    <template #title>
                    <div><van-icon name="qr" /> 首尔 </div>
                    </template>
                    内容
                </van-collapse-item>
                <van-collapse-item title="首页二" name="2" icon="shop-o">
                    内容
                </van-collapse-item>
            </van-collapse>
        </div>
    </template>

    <script>
    export default {
        name:'sidernav',
        data() {
            return {
                activeNames: '1',//此时activeName为字符串格式
            };
        },
        created() {

        },
        mounted() {

        },
        methods: {

        }
    };
</script>

<style scoped lang="less">

</style>


``` 