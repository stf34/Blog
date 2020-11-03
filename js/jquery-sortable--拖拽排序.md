## jquery-sortable--拖拽排序
> SortableJS 中文网址: http://www.sortablejs.com/
> 借鉴文档： http://www.itxst.com/sortablejs/neuinffi.html

### 拖拽排序

> 别的不多说了，直接上例子

```html
    <!doctype html>
    <html>

    <head>
        <meta charset="utf-8">
        <title>Sortable jQuery</title>
        <script src="http://www.itxst.com/package/sortable/sortable.min.js"></script>
        <style>
            body.dragging,
            body.dragging * {
                cursor: pointer !important;
            }

            .dragged {
                position: absolute;
                opacity: 0.5;
                z-index: 2000;
                cursor: pointer;
            }

            ol.example li.placeholder {
                position: relative;
            }

            ol.example li.placeholder:before {
                position: absolute;
            }

            .max_phone {
                display: flex;
                justify-content: space-around;
            }

            .example li {
                cursor: pointer;
            }
        </style>
    </head>

    <body>
        <h3>点击下方拖拽排序</h3>
        <div class="max_phone">
            <ol class='examples' id="examples">
                <li>Dome-01</li>
                <li>Dome-02</li>
                <li>Dome-03</li>
                <li>Dome-04</li>
                <li>Dome-05</li>
            </ol>
        </div>
        <script>
            $(function () {
                Sortable.create(document.getElementById('examples'), {
                    animation: 150, //动画参数
                    onAdd: function (evt) { //拖拽时候添加有新的节点的时候发生该事件
                        console.log('onAdd.foo:', [evt.item, evt.from]);
                    },
                    onUpdate: function (evt) { //拖拽更新节点位置发生该事件
                        console.log('onUpdate.foo:', [evt.item, evt.from]);
                    },
                    onRemove: function (evt) { //删除拖拽节点的时候促发该事件
                        console.log('onRemove.foo:', [evt.item, evt.from]);
                    },
                    onStart: function (evt) { //开始拖拽出发该函数
                        console.log('onStart.foo:', [evt.item, evt.from]);
                    },
                    onSort: function (evt) { //发生排序发生该事件
                        console.log('onSort.foo:', [evt.item, evt.from]);
                    },
                    onEnd: function (evt) { //拖拽完毕之后发生该事件
                        var itemEl = evt.item;  // 拖拽的元素
                        console.log(evt.to);//推拽之后的目标列表
                        console.log(evt.from);//推拽之前的目标列表
                        console.log(evt.oldIndex);//拖拽之前的索引值
                        console.log(evt.newIndex);//拖拽之后的新的索引值
                        console.log(evt.clone);//这是克隆的元素
                        console.log(evt.pullMode);//当项目在另一个可排序中时：如果是克隆，则为“克隆”；如果是移动，则为“true”
                    }
                });
            })
        </script>
    </body>

    </html>
```

### 从A列表克隆到B列表

```html
    <!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8" />
    <title>sortable.js 拖拽时从A组克隆到B组例子</title>
    <meta name="viewport"
        content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no, minimal-ui">
    <script src="http://www.itxst.com/package/sortable/sortable.min.js"></script>
    <style>
        .box {}

        .itxst {
            margin: 10px auto;
            width: 30%;
            float: left;
            margin-right: 10px;
        }

        .itxst div {
            padding: 6px;
            background-color: #fdfdfd;
            border: solid 1px #eee;
            margin-bottom: 10px;
            cursor: move;
        }

        #msg {
            clear: both;
            width: 100%;
        }
    </style>
</head>

<body>
    <div class="box">
        <div id="g1" class="itxst">
            <div class="item" data-id="1">item 1</div>
            <div class="item" data-id="2">item 2</div>
            <div class="item" data-id="3">item 3</div>
        </div>
        <div id="g2" class="itxst">
            <div class="item" data-id="4">www.itxst.com</div>
            <div class="item" data-id="5">www.google.com</div>
            <div class="item" data-id="6">www.baidu.com</div>
        </div>
    </div>
    <div id="msg"></div>
    <script>
        //第一组
        var g1 = document.getElementById('g1');
        var ops1 = {
            animation: 1000,
            draggable: ".item",
            group: { name: "itxst.com", pull: 'clone', put: false },
            //拖动结束
            onEnd: function (evt) {
                // console.log(evt.target.children);

            },
        };
        var sortable1 = Sortable.create(g1, ops1);
        //第二组
        var g2 = document.getElementById('g2');
        var ops2 = {
            animation: 1000,
            draggable: ".item",
            group: { name: "itxst.com", pull: true, put: true },
            //拖动结束
            onEnd: function (evt) {
                console.log(evt);
                //获取拖动后的排序
                var arr = sortable2.toArray();
                document.getElementById("msg").innerHTML = "B组排序结果：" + JSON.stringify(arr);
            },

            onAdd: function (evt) { //拖拽时候添加有新的节点的时候发生该事件
                var itemEl = evt.item;  // 拖拽的元素
                console.log(evt.to);//推拽之后的目标列表
                console.log(evt.from);//推拽之前的目标列表
                console.log(evt.oldIndex);//拖拽之前的索引值
                console.log(evt.newIndex);//拖拽之后的新的索引值
                console.log(evt.clone);//这是克隆的元素
                console.log(evt.pullMode);//当项目在另一个可排序中时：如果是克隆，则为“克隆”；如果是移动，则为“true”
            },



        };
        var sortable2 = Sortable.create(g2, ops2);

    </script>
</body>

</html>
```

### 左侧，右侧相互联动拖动

```html
    <!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>sortable.js例子</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no, minimal-ui">
    <script src="http://www.jq22.com/jquery/jquery-1.10.2.js"></script>
    <script src="js/Sortable.min.js"></script>
    <style>
        .dome{
            display: flex;
            justify-content: space-around;

        }
        #itxst,#itxsts {
            margin: 10px auto;
            width: 80%;
        }

        #itxst div,#itxsts div {
            padding: 6px;
            background-color: #fdfdfd;
            border: solid 1px #eee;
            margin-bottom: 10px;
            cursor: move;
        }
    </style>
</head>
<body>
    <div class="dome">
        <!-- 左侧 -->
        <div id="itxst" class="itxst">
        
        </div>
        <!-- 右侧 -->
        <div id="itxsts" class="itxst">
    
        </div>
    </div>
    <script>
        var data = ['item1','item2','item3']
        leftSort()
        rightSort()
        devDom ()
        
        function devDom (){
            var dev = ``;
            data.forEach((element,eleIndex) => {
                dev+=`<div class='item' data-id="${eleIndex}">${element}</div>`;
            });
            $('#itxst').html(dev)
            $('#itxsts').html(dev)
        }
        function leftSort(){
            //获取对象
            var el = document.getElementById('itxst');
            //设置配置
            var ops = {
                animation: 1000,
                draggable: ".item",
                group: { name: "itxst.com", pull: true, put: true },
                //拖动结束
                onEnd: function (evt) {

       
                    var documentType = [];
                    var domArr = [];

                    /* 伪数组，转换 */
                    domArr = Array.from(evt.target.children)
                    //获取每个节点最初的位置索引；根据索引值所在当前位置重新排序；
                    domArr.forEach((item, index) => {
                        documentType.push(item.dataset.id);
                    });

                    var Arry = [];
                    for (let i = 0; i < documentType.length; i++) {
                        Arry.push(data[documentType[i]]);
                    }

                    data = Arry;

                    //  更新dom数中的索引值；
                    domArr.forEach((item, index) => {
                        item.dataset.id = index;
                    });

                    devDom()
                },
            };
            //初始化
            var sortable = Sortable.create(el, ops);
        }
        function rightSort(){
            //获取对象
            var el = document.getElementById('itxsts');
            //设置配置
            var ops = {
                animation: 1000,
                draggable: ".item",
                group: { name: "itxst.com", pull: 'clone', put: false },
                //拖动结束
                onEnd: function (evt) {


                    var documentType = [];
                    var domArr = [];
  
                    /* 伪数组，转换 */
                    domArr = Array.from(evt.target.children)
                    //获取每个节点最初的位置索引；根据索引值所在当前位置重新排序；
                    domArr.forEach((item, index) => {

                        documentType.push(item.dataset.id);
                    });

                    var Arry = [];
                    for (let i = 0; i < documentType.length; i++) {
                        Arry.push(data[documentType[i]]);
                    }

                    data = Arry;

                    //  更新dom数中的索引值；
                    domArr.forEach((item, index) => {
                        item.dataset.id = index;
                    });

                    devDom()
                },
            };
            //初始化
            var sortable = Sortable.create(el, ops);
        }
    </script>
</body>
</html>
```