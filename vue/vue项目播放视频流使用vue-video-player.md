### vue 播放器插件 之 vue-video-player
> 安装依赖
    ``` js
        npm install vue-video-player --save
    ```
> 项目中使用,
1. 先在`main.js`里引入
    ```js
        // 引入videojs
        import VideoPlayer from 'vue-video-player'
        import 'vue-video-player/src/custom-theme.css'
        import 'video.js/dist/video-js.css'
        
        Vue.use(VideoPlayer)

    ```
2. 组件中使用
    * html中样式
    ```html
        <template>
            <div class="demo">
                <div class="Btn" @click="toD()">点击跳转直播间</div>
                <video-player
                class="video-player vjs-custom-skin"
                ref="videoPlayer"
                :playsinline="true"
                :options="playerOptions"
                @playing="onPlayerPlaying($event)"
                ></video-player>
            </div>
        </template>

    ```
    * js 逻辑
    ```js
        export default {
            props: ["videoSrc"],
            data() {
                return {
                videoUrl: this.videoSrc,
                // 视频播放
                playerOptions: {
                    playbackRates: [0.7, 1.0, 1.5, 2.0], //播放速度
                    autoplay: true, //如果true,浏览器准备好时开始回放。
                    muted: true, // 默认情况下将会消除任何音频。
                    loop: false, // 导致视频一结束就重新开始。
                    preload: "auto", // 建议浏览器在<video>加载元素后是否应该开始下载视频数据。auto浏览器选择最佳行为,立即开始加载视频（如果浏览器支持）
                    language: "zh-CN",
                    aspectRatio: "16:9", // 将播放器置于流畅模式，并在计算播放器的动态大小时使用该值。值应该代表一个比例 - 用冒号分隔的两个数字（例如"16:9"或"4:3"）
                    fluid: true, // 当true时，Video.js player将拥有流体大小。换句话说，它将按比例缩放以适应其容器。
                    sources: [
                    {
                        type: "", // 类型
                        //src: "http://vjs.zencdn.net/v/oceans.mp4" //url地址
                        src: "" //动态修改url地址时，可以为空
                        //src: require('./video.mp4') //引入本地视频，因为引入的是本地资源，要把资源写在“require”方法里
                    }
                    ],
                    poster: "", //你的封面地址
                    // width: document.documentElement.clientWidth,
                    notSupportedMessage: "此视频暂无法播放，请稍后再试", //允许覆盖Video.js无法播放媒体源时显示的默认信息。
                    controlBar: {
                    timeDivider: true,
                    durationDisplay: true,
                    remainingTimeDisplay: false,
                    fullscreenToggle: true //全屏按钮
                    }
                }
                };
            },
            watch: {
                /* 监听传的值 */
                videoSrc(newval, oldval) {
                /* 将变化的新值，赋予到要渲染的值 */
                this.videoUrl = newval;

                }
            },
            mounted() {
                this.playerOptions['sources'][0]['src'] = this.videoUrl;//动态修改播放地址网址
                this.playerOptions['sources'][0]['src'] = require(`${this.video}`)//动态修改播放本地视频
            },
            computed: {
                player() {
                return this.$refs.videoPlayer.player;
                }
            },
            methods: {
                /* 播放时的事件 */
                onPlayerPlaying($event) {
                console.log($event);
                },
                toD() {
                window.location.href="https://www.baidu.com";
                }
            }
        };
    ```
    * css 样式
    ``` css
        .demo {
            display: inline-block;
            width: 600px;
            height: 338px;
            text-align: center;
            line-height: 100px;
            border: 1px solid transparent;
            border-radius: 4px;
            overflow: hidden;
            background: #fff;
            position: relative;
            box-shadow: 0 1px 1px rgba(0, 0, 0, 0.2);
            margin-right: 4px;
            position: relative;
        }
        .Btn {
            position: absolute;
            top: 44%;
            left: 24%;
            background-color: #fff;
            opacity: 0.6;
            z-index: 999;
            width: 120px;
            height: 40px;
            line-height: 40px;
        }
        .demo:hover {
            display: block;
        }
    ```

### 在组件中使用

``` js
    import videSP from "./components/video";//引入视频组件
    export default {
        components: {
            videSP
        },

        data() {
            return {
                videoSrc:'http://vjs.zencdn.net/v/oceans.mp4'//动态传播的视频地址
            };
        },
    }
```