<template>
  <div class="home">
    <!-- 头部组件 -->
    <HomeHeader />
    <Son1 :a="[1, 2, 3]" @c="fn" @input="changV" :value="value"></Son1>
    <!-- 轮播图 -->
    <!-- <cube-slide :data="sliders" />
    <Son :money.sync="money"></Son>

    <component v-bind:is="currentcom"></component>
    <button @click="changc">改变组件</button>
    <!-- 列表  -->
    <!-- <div class="view-wrapper">
      <cube-recycle-list
        class="list"
        :size="size"
        :on-fetch="onFetch"
        :offset="offset"
        ref="list"
      >
        <template slot="item" slot-scope="{ data }">
          <div :id="data.id" class="item" @click="handleClick(data)">
            <div>{{ data.price | money("$") }}</div>
          </div>
        </template>
      </cube-recycle-list>
    </div> -->
  </div>
</template>

<script>
// @ is an alias to /src
import { createNamespacedHelpers } from "vuex";
import * as Types from "../store/mutations-types";
import HomeHeader from "../components/HomeHeader.vue";
import Son from "./Son.vue";
import Son1 from "./Son1.vue";

const { mapActions, mapState } = createNamespacedHelpers("home");

export default {
  name: "home",
  provide() {
    return { msg: "provide" };
  },
  components: {
    HomeHeader,
    Son,
    Son1
  },
  created() {
    this[Types.SET_SLIDER]();
  },
  computed: {
    ...mapState(["sliders"])
  },
  data() {
    return {
      currentcom: "Son",
      money: 100,
      size: 20, // 一次拉取多少条数据
      offset: 100,
      slide: [],
      value: 300000
    };
  },
  methods: {
    changV(val) {
      this.value = val;
    },
    fn() {
      alert("我是fn");
    },
    changc() {
      this.currentcom = "Son1";
    },
    ...mapActions([Types.SET_SLIDER]),
    onFetch() {
      // 到底部的时候触发的事件 会自动线调用一次

      const slide = [];
      return new Promise(resolve => {
        // 模拟请求 50 条数据，因为 size 设置为 50
        setTimeout(() => {
          // eslint-disable-next-line no-plusplus
          for (let i = 0; i < 50; i++) {
            slide.push({
              id: i,
              avatar:
                "https://s3.amazonaws.com/uifaces/faces/twitter/danpliego/128.jpg",
              price: "123",
              time: "列表"
            });
          }
          resolve(slide);
        }, 1000);
      });
    },
    handleClick(data) {
      // eslint-disable-next-line no-multi-spaces
      console.log(`Item:${data}`);
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.$bus.$on("bus", data => {
        console.log(data);
      });
    }); // dom更新完成
    // 给列表绑定滚动事件
    // this.$refs.list.$el.addEventListener('scroll', () => {
    //   let timer;
    //   if (timer) {
    //     clearTimeout(timer);
    //   } else {
    //     timer = setTimeout(() => {
    //       // 拿到当前滚动条的高度
    //       sessionStorage.setItem('listTop', this.$refs.list.$el.scrollTop);
    //     }, 500);
    //   }
    // });
  },
  activated() {
    // keepAlive的组件生命周期
    // const listTop = sessionStorage.getItem('listTop');
    // this.$refs.list.scrollTop = listTop || 0;
  }
};
</script>
<style lang="less" scoped>
.view-wrapper {
  position: fixed;
  width: 100%;
  height: calc(100vh - 210px);
}
</style>
