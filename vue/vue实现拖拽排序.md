## vue实现拖拽排序
> Draggable为基于Sortable.js的vue组件，用以实现拖拽功能。
> vue.draggable中文文档： http://www.itxst.com/vue-draggable/tutorial.html


1. 安装：
> `npm i -S vuedraggable`

2. 引入：
> `npm i -S vuedraggable`


> 我们可以将其封装成一个组件`index.vue`，然后在其他页面使用
```vue
    <template>
        <div>
            <div class="drag">
                <draggable
                    :move="getdata"
                    @update="datadragEnd"
                    class="dragUl"
                    v-model="list">
                    <div class="dragLi" :key="index" v-for="(item, index) in list">
                        <p class="Text">
                            {{ item }}
                        </p>
                        <img class="Icon" src="../../assets/img/icon/shuye.png" alt="" srcset="">
                    </div>
                </draggable>
            </div>
        </div>
    </template>
```
> js 逻辑
```js
        import draggable from "vuedraggable";
        export default {
            props:['ListData'],//传递出来的数据
            data() {
                return {
                    list: this.ListData
                };
            },
            components: {
                draggable
            },
            created() {

            },
            watch: {
                /* 监听传的值 */
                // ListData(newval, oldval) {
                //     /* 将变化的新值，赋予到要渲染的值 */
                //     console.log('监听传的值',newval, oldval);
                //     // this.details = newval.replace(/\n/g, "<br/>");
                //     // this.detailsData = newval.replace(/\n/g, "<br/>");
                //     console.log(12313);
                // }
            },
            mounted() {
                //为了防止火狐浏览器拖拽的时候以新标签打开，此代码真实有效
                document.body.ondrop = function (event) {
                    event.preventDefault();
                    event.stopPropagation();
                }
            },
            methods: {
                getdata(evt) {
                    console.log(evt);
                    //这里evt后续的内容根据具体的定义变量而定
                },
                datadragEnd(evt) {
                    console.log("拖动前的索引 :" + evt.oldIndex);
                    console.log("拖动后的索引 :" + evt.newIndex);
                    /* this.$store.commit 更改Vuex里面的值 */
                    this.$store.commit(
                        'changeStepsNumber',
                        {
                            Number:evt.newIndex
                        }
                    )
                },
            }
        };
```
> css样式
```css
    .dragUl {
        overflow: hidden;
        .dragLi {
            position: relative;
            height: auto;
            background-color: #21214D;
            opacity: 0.6;
            color: #fff;
            margin-bottom: 10px;
            text-align: center;
            border-radius: 15px;
            box-sizing: border-box;
            padding: 20px;
            .Icon{
                position: absolute;
                top: 0;
                right: 0;
                width: 40px;
                height: 40px;
            }
            .Text{
                word-wrap:break-word
            }
        }
    }
```
> 最后在组件中使用
```js
    import valSoll from '../draggable/index'
    export default {
        components:{
            valSoll
        },
    };
```