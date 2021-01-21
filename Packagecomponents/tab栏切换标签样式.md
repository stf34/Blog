## 封装tab栏切换改变内容并居中当前点击标签
> 主要使用了父子组件之间传值，使用发布和订阅，`watch`监听让父子组件之间实现双向绑定，从而实现操作子组件也引起父组件内的数据变化，如果想要实现滑动内容切换tab标签，可以结合`swiper`实现

### 标签封装属性
  1. topNavheight：标签盒子的高度
  3. currentcolor：标签字体颜色
  4. currentfotsize：标签字体字号
  5. subbgcolor：标签下标选中颜色

> 因为有些写的是自适应，所以就没有在封装了,如果想要改变标签显示的数量可以在组件内修改(如果在外修改可能造成点击居中失败)

下面就是主要代码逻辑了
```vue
<template>
  <div class="assort">
    <div class="topNav" id="topNav" :style="{ height: topNavheight }">
      <ul id="topNavUl">
        <li
          v-for="(i, index) in title"
          :key="index"
          @click="goCenter(index, $event)"
          :class="{ current: index === currentTopNav }"
          :style="{
            'line-height': topNavheight,
            color: index === currentTopNav ? currentcolor : '#000',
            'font-size': currentfotsize,
          }"
        >
          {{ i.name }}
          <span
            class="sub"
            :style="{
              'background-color': index === currentTopNav ? subbgcolor : '#000',
            }"
          ></span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "",
  data() {
    return {
      currentTopNav: 0, //点击当前内容
      topNavheight: this.Navheight, //标签盒子高度
      currentcolor: this.color, //标签字体颜色
      currentfotsize: this.fotsize, //标签字号
      subbgcolor: this.bgcolor, //标签下标字体颜色颜色
      title: this.Item,
    };
  },
  props: ["Item", "Navheight", "color", "fotsize", "bgcolor"],
  components: {},
  mounted() {},
  watch: {
    /* 监听父组件传递内容变化并实时更新 */
    title(newVal, oldVal) {
      this.title = newVal;
    },
    /* 监听当前标签下标并实时更新父组件内容变化 */
    currentTopNav(newVal, oldVal) {
      this.$emit("active", newVal);
    },
    /* 监听父组件传递高变化并实时更新 */
    topNavheight(newVal, oldVal) {
      this.topNavheight = newVal;
    },
    /* 监听父组件传递内容字体颜色变化并实时更新 */
    currentcolor(newVal, oldVal) {
      this.currentcolor = newVal;
    },
    /* 监听父组件传递字号变化并实时更新 */
    currentfotsize(newVal, oldVal) {
      this.currentfotsize = newVal;
    },
    /* 监听父组件传递下标变化并实时更新 */
    subbgcolor(newVal, oldVal) {
      this.subbgcolor = newVal;
    },
  },
  methods: {
    //点击切换，居中
    goCenter(index, e) {
      /* 点击切换点击中的内容 */
      this.currentTopNav = index;
      // 横向滑动居中
      let ul = document.querySelector("#topNavUl"); //盒子内标签的样式
      let nav = document.getElementById("topNav"); //外层盒子
      let window_offsetWidth = window.innerWidth; //屏幕宽度;
      let dom = e.target;

      /* getBoundingClientRect用于获取某个元素相对于视窗的位置集合。
            集合中有top, right, bottom, left等属性。
        */
      /* 
            通过点击的当前元素来判断是否让元素居中，
            如果是首尾的几个元素就让其在初始位置呆着，
            如果是正常的元素就让其进行偏移居中
         */
      if (dom) {
        let domoffsetWidth = dom.offsetLeft,
          //中间值 =( 屏幕宽度 - li宽度 ) / 2;
          diffWidth = (window_offsetWidth - dom.offsetWidth) / 2,
          //目标值 = offset - 中间值
          targetOffset = -(domoffsetWidth - diffWidth);
        let ul_width = ul.getBoundingClientRect().width; //开始

        // 未超出中间值
        if (-targetOffset < 0) {
          nav.scrollLeft = 0;
          return;
        }
        //末尾
        if (targetOffset < window_offsetWidth - ul_width) {
          nav.scrollLeft = -(window_offsetWidth - ul_width);
          return;
        }
        //正常
        nav.scrollLeft = -targetOffset;
      }
    },
  },
};
</script>

<style lang="less" scoped>
.topNav {
  // width: 100%;
  height: 1rem;
  background: #fff;
  display: flex;
  flex-flow: row nowrap;
  overflow: scroll;
  ul {
    // width: 100%;
    display: inline-block;
    white-space: nowrap;
    li {
      display: inline-block;
      width: 1.8rem;
      line-height: 1rem;
      font-size: 0.32rem;
      text-align: center;
      box-sizing: border-box;
      padding: 0 0.1rem;
      margin: 0 0.1rem;
      &.current {
        position: relative;
        color: #000;
        .sub {
          display: block;
          width: 30%;
          height: 0.08rem;
          border-radius: 0.1rem;
          background-color: #e73462;
          position: absolute;
          bottom: 0.15rem;
          right: 35%;
        }
      }
    }
  }
}
</style>
```

### 父组件内使用
> 先引入，然后根据属性值去操作子组件的变化

```vue
<template>
  <div>
    <tablable
      :Item="title"
      :Navheight="topNavheight"
      :width="currentwidth"
      :color="currentcolor"
      :fotsize="currentfotsize"
      :bgcolor="subbgcolor"
      @active="activelable"
    />
  </div>
</template>

<script>
import tablable from "./tablable";
export default {
  name: "",
  data() {
    return {
      activ: 0,
      topNavheight: "1rem", //标签盒子高度
      currentwidth: "25%", //标签宽度
      currentcolor: "#66cc66", //标签字体颜色
      currentfotsize: "0.32rem", //标签字号
      subbgcolor: "#66cc66", //标签下标字体颜色颜色
      title: [
        {
          name: "标签一",
        },
        {
          name: "标签二",
        },
        {
          name: "标签三",
        },
        {
          name: "标签四",
        },
        {
          name: "标签五",
        },
        {
          name: "标签六",
        }
      ],
    };
  },

  components: {
    tablable,
  },
  mounted() {},
  methods: {
    activelable(val) {
      this.activ = val;
    },
  },
  watch: {
    activ(newVal, old) {
      console.log(`父组件`, newVal);
      /* 这里放置你要实现的代码逻辑    */
    },
  },
  // computed: {
  //   activelable(val) {
  //     console.log(val);
  //   },
  // },
};
</script>

<style scoped>
</style>
```