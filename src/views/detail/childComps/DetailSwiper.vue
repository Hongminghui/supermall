<template>
  <div>
<!--    <swiper class="detail-swiper">-->
<!--      <swiper-item v-for="(item, index) in imageList" :key="index">-->
<!--        <img :src="item" alt="">-->
<!--      </swiper-item>-->
<!--    </swiper>-->
    <!--此处运用element-ui-->
    <el-carousel  class="detail-swiper" ref="carousel">
      <el-carousel-item v-for="item in imageList" :key="item" class="item">
        <img :src="item" alt="">
      </el-carousel-item>
    </el-carousel>
  </div>
</template>

<script>

  export default {
    name: "DetailSwiper",
    props: {
      imageList: {
        type: Array,
        default() {
          return []
        }
      }
    },

    methods: {

    },
    components: {

    }
  }
</script>

<style scoped>
  .detail-swiper {
    height: 300px;
    width: 100%;
  }
  .item {
    text-align: center;
    height: 300px;
  }
  img {
   width: 100%;
  }


</style>
