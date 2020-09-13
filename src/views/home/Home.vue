<template>
  <div class=".page-component" id="home" >


    <nav-bar class="home-nav-bar">
      <div slot="center">购物街</div>
    </nav-bar>
    <!--此处使用了element-ui-->
    <!--监听滚轮事件要放在这-->
    <!--$event是指当前触发的是什么事件（鼠标事件，键盘事件等）
$       event.target则指的是事件触发的目标，即哪一个元素触发了事件，这将直接获取该dom元素-->
    <div class="infinite-list-wrapper content" style="overflow:auto"
         @scroll="tabScroll($event)"
         ref="scrollWrapper">
      <ul
        class="list"
        v-infinite-scroll="load"
        infinite-scroll-disabled="disabled">
        <home-swiper :banners="banners" class="home-swiper"></home-swiper>
        <home-recommend :recommends="recommends" class="recommend"></home-recommend>
        <feature class="feature"></feature>
        <tab-control :titles="['流行', '新款', '精选']"
                     ref="tabControl"
                     :class="{'is_fixed' : isFixed}"
                     @tabClick="tabClick"
                     >
        </tab-control>
        <goods-list :goods="showGoods" ref="goodsList" ></goods-list>

      </ul>
      <p v-if="loading">加载中...</p>
      <p v-if="noMore">没有更多了</p>
    </div>
    <!--此处使用了element-ui，实现返回顶部的功能-->
    <el-backtop target=".content" :bottom="100" :right="20" class="backTop">
      <div class="backTop">
        <i class="el-icon-top" ></i>
      </div>
    </el-backtop>

  </div>
</template>

<script>
  /*子组件*/
  import HomeSwiper from "./childComps/HomeSwiper";
  import HomeRecommend from "./childComps/HomeRecommend";
  import Feature from "./childComps/Feature";
  import MainTabBar from "../../components/content/mainTabbar/MainTabBar";
  /*公共组件*/
  import NavBar from "../../components/common/navbar/NavBar";
  import TabControl from "../../components/content/tabControl/TabControl";
  import GoodsList from "../../components/content/goods/GoodsList";
  import Scroll from "../../components/common/scroll/Scroll";

  /*网络请求*/
  import {getHomeGoods, getMultiData} from "../../network/home";

  export default {
    name: "Home",
    components: {
      HomeSwiper,
      HomeRecommend,
      Feature,
      Scroll,
      MainTabBar,

      NavBar,
      TabControl,
      GoodsList
    },
    data() {
      return {
        //方便查看devTools里的数据
        dataResult: null,
        banners: [],
        recommends: [],
        goods: {
          'pop': {page: 0, list: []},
          'new': {page: 0, list: []},
          'sell': {page: 0, list: []}
        },
        goodsResult: null,
        currentType: 'pop',
        isShowBackTop: false,
        loading: false,
        isFixed: false,
        scrollTop: 0,
        saveY: 0
      }
    },
    created() {
      this.getMultiData()
      this.getHomeGoods('pop')
      this.getHomeGoods('new')
      this.getHomeGoods('sell')
    },
    activated() {
      this.$refs.scrollWrapper.scrollTop = this.saveY
    },
    deactivated() {
      //离开home路由时，记录下滚动的距离
      this.saveY = this.scrollTop
    },
    destroyed() {
      console.log('destroyed')
    },
    methods: {
      /*网络请求*/
      getMultiData(){
        getMultiData().then(res => {
          this.DataResult = res
          this.banners = res.data.banner.list
          this.recommends = res.data.recommend.list
        })
      },
      getHomeGoods(type){
        const page = this.goods[type].page + 1
        getHomeGoods(type, page).then(res => {
          this.goodsResult = res
          this.goods[type].list.push(...res.data.list)
          this.goods[type].page += 1
        })
      },
      /*事件监听*/
      tabClick(index) {
        switch (index) {
          case 0:
            this.currentType = 'pop'
            break
          case 1:
            this.currentType = 'new'
            break
          case 2:
            this.currentType = 'sell'
        }
      },
      load() {
        this.loading = true
        setTimeout(() => {
          this.getHomeGoods(this.currentType)
          this.loading = false
        }, 2000)
      },
      tabScroll(event) {
        //event.target 触发事件的对象，即this.$refs.scrollWrapper
        // this.scrollTop = event.target.scrollTop
        this.scrollTop = this.$refs.scrollWrapper.scrollTop
        //如果被卷曲的高度大于吸顶元素到顶端位置 的距离
        this.isFixed = this.scrollTop > this.$refs.tabControl.$el.offsetTop;
      }
    },
    computed: {
      showGoods() {
        return this.goods[this.currentType].list
      },
      noMore () {
        return !this.goodsResult
      },
      disabled () {
        return this.loading || this.noMore
      },

    }
  }
</script>

<style scoped>
  #home {
    position: relative;
    height: 100vh;
  }
  .home-nav-bar{
    background-color: var(--color-tint);
    position: fixed;
    top: 0;
    z-index: 9;
    width: 100%;
  }
  .el-icon-top {
    height: 90%;
  }
  tab-control {
    position: fixed;
    top: 44px;
    z-index: 9;
  }
  .infinite-list-wrapper {
    top: 44px;
    overflow: hidden;
    position: absolute;
    bottom: 49px;
    left: 0;
    right: 0;
  }
  .backTop {
    font-size: 30px;
    z-index: 9;
  }
  .is_fixed{
    position: fixed;
    top: 44px;
    width: 100%;
    background-color: white;
    z-index: 9;
  }
</style>
