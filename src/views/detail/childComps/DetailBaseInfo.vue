<template>
  <div class="baseInfo">
    <!--让里面的元素有内边距-->
    <div class="padding">
      <div class="price">
        <span class="newPrice">{{baseInfo.newPrice}}</span>
        <span class="oldPrice">{{baseInfo.oldPrice}}</span>
        <span class="discount">{{baseInfo.discount}}</span>
      </div>
      <div class="title">{{baseInfo.title}}</div>
    </div>
    <!--让两个块中间有一条空白-->
    <div class="blank"></div>
    <!--让里面元素有内边距-->
    <div class="padding">
      <div class="services">
        <div v-for="(item, index) in baseInfo.services" class="servicesItem">
          <img :src="item.icon" alt="">
          <span>{{item.name}}</span>
        </div>
      </div>
    </div>
    <div class="blank"></div>

  </div>
</template>

<script>
  export default {
    name: "detailBaseInfo",
    props: {
      baseInfo: {
        type: Object
      }
    }
  }
</script>

<style scoped>
  .baseInfo {
    margin-top: 15px;
    color: black;
    border-bottom: 5px solid #f2f5f6;
    margin-bottom: 0px;
  }
  .padding {
    padding:0 10px;
  }
  .price {
    margin-bottom: 8px;
    padding: 5px;
  }
  .newPrice {
    font-size: 28px;
  }
  .oldPrice {
    font-size: 12px;
    color: #666666;
    text-decoration: line-through;
    position: relative;
    top: -14px;
  }
  .discount {
    font-size: 12px;
    background-color: rgb(255,232,238);
    position: relative;
    left: -30px;
    top: 5px;
  }
  .title {
    font-size: 15px;
    padding-right: 30px;
    padding-bottom: 15px;
    padding-left: 5px;
  }
  .services {
    margin-top: 5px;
    display: flex;
    flex-wrap: wrap;
    font-size: 14px;
    padding: 5px;
    margin-bottom: 5px;
  }
  .servicesItem {
    margin-right: 10px;
    height: 15px;
  }
  .services img {
    width: 12px;
  }
  .blank {
    height: 10px;
    line-height: 10px;
    background-color: rgb(234, 234, 234);
  }
</style>
