<template>
  <div>
    <detail-nav-bar @navItemClick="themeScroll" :current-index="currentIndex"></detail-nav-bar>
    <div class="infinite-list-wrapper content"
         style="overflow:auto"
         @scroll="scrollTheme" ref="scrollWrapper">
      <ul
        class="list"
        v-infinite-scroll=""
        infinite-scroll-disabled="disabled">
        <detail-swiper :image-list="topImage"></detail-swiper>
        <detail-base-info :base-info="baseInfo"></detail-base-info>
        <detail-shop-info :shop-info="shopInfo"></detail-shop-info>
        <detail-goods-info :detail-info="detailInfo" ref="goods"></detail-goods-info>
        <detail-param-info :param-info="paramInfo" ref="param"></detail-param-info>
        <detail-comment-info :comment-info="commentInfo" ref="comment"></detail-comment-info>
      </ul>
    </div>
    <el-backtop :bottom="100" :right="20" class="backTop" target=".content">
    </el-backtop>
    <detail-bottom-bar class="bottom" @addToCart="addToCart"></detail-bottom-bar>
  </div>
</template>

<script>
  //子组件
  import DetailNavBar from "./childComps/DetailNavBar";
  import DetailSwiper from "./childComps/DetailSwiper";
  import DetailBaseInfo from "./childComps/DetailBaseInfo";
  import DetailShopInfo from "./childComps/DetailShopInfo";
  import DetailGoodsInfo from "./childComps/DetailGoodsInfo";
  import DetailParamInfo from "./childComps/DetailParamInfo";
  import DetailBottomBar from "./childComps/DetailBottomBar";
  import DetailCommentInfo from "./childComps/DetailCommentInfo";

  //网络相关
  import {getDetail} from "../../network/detail";

  export default {
    name: "Detail.vue",
    data() {
      return {
        iid: '111',
        topImage: [],
        detailResult: null,
        //将baseInfo需要的数据封装到一个对象中，便于传递数据，必须有内容（discount: null），
        // 不能是空对象{}，不然页面上不会显示
        baseInfo: {discount: null},
        shopInfo: null,
        detailInfo: {},
        paramInfo: {image: null},
        commentInfo: {},
        themeTops: [],
        currentIndex: 0
      }
    },
    methods: {
      //得到各个主题的offsetTop
      getOffsetTop() {
        this.themeTops.push(this.$refs.goods.$el.offsetTop)
        this.themeTops.push(this.$refs.param.$el.offsetTop)
        this.themeTops.push(this.$refs.comment.$el.offsetTop)
      },

      //点击导航栏中标题，滚动到对应主题
      themeScroll(index) {
        this.currentIndex = index
        this.$refs.scrollWrapper.scrollTop = this.themeTops[index]
      },
      //滚动到某一主题的内容时，导航栏也切换到对应主题
      scrollTheme() {
        let currentPosition = this.$refs.scrollWrapper.scrollTop
        if(currentPosition < this.themeTops[1]){
          this.currentIndex = 0
        }else if(currentPosition < this.themeTops[2]){
          this.currentIndex = 1
        }else {
          this.currentIndex = 2
        }
      },
      addToCart() {
        // 1.创建对象
        const obj = {}
        // 2.对象信息
        obj.iid = this.iid;
        obj.imgURL = this.topImage[0]
        obj.title = this.baseInfo.title
        obj.desc = this.baseInfo.desc;
        obj.newPrice = this.baseInfo.nowPrice;
        console.log('success')
        this.$store.commit('addCart', obj)
      },
      //获取接口中的数据
      getDetailData() {
        const iid = this.$route.params.iid
        this.iid = iid
        getDetail(iid).then(res => {
          this.detailResult = res
          const data = res.result

          //获取轮播图
          this.topImage = data.itemInfo.topImages

          //获取商品基本信息
          this.baseInfo.title = data.itemInfo.title
          this.baseInfo.desc = data.itemInfo.desc
          this.baseInfo.newPrice = data.itemInfo.price
          this.baseInfo.oldPrice = data.itemInfo.oldPrice
          this.baseInfo.nowPrice = data.itemInfo.highNowPrice
          this.baseInfo.discount = data.itemInfo.discountDesc
          this.baseInfo.services = data.shopInfo.services
          this.baseInfo.columns = data.columns

          //获取店铺信息
          this.shopInfo = data.shopInfo

          //获取商品信息
          this.detailInfo = data.detailInfo

          //获取商品参数信息
          this.paramInfo.image = data.itemParams.info.images ? data.itemparams.info.images[0] : '[]';
          this.paramInfo.infos = data.itemParams.info.set;
          this.paramInfo.sizes = data.itemParams.rule.tables;

          //获取评论信息
          if(data.rate.list) {
            this.commentInfo = data.rate.list[0]
          }

        })
      }
    },
    mounted() {
      console.log('created')
      this.getDetailData()
    },
    updated() {
      this.getOffsetTop()
    },

    components: {
      DetailNavBar,
      DetailSwiper,
      DetailBaseInfo,
      DetailShopInfo,
      DetailGoodsInfo,
      DetailParamInfo,
      DetailBottomBar,
      DetailCommentInfo,
    },
  }
</script>

<style scoped>
  .infinite-list-wrapper {
    position: absolute;
    top: 44px;
    height: calc(100% - 44px - 52px);
  }
  .backTop {
    text-align: center;
    font-size: 30px;
  }
  .bottom {
    z-index: 99;
  }
</style>
