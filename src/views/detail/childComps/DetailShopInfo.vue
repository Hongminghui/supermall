<template>
  <div class="shop-info">

    <div class="base-info">
      <div class="logo">
        <img :src="shopInfo.shopLogo" alt="">
      </div>
      <div class="info">
        <div>{{shopInfo.name}}</div>
        <div>在架商品{{shopInfo.cGoods}}</div>
        <div class="sell">
          <span class="sell-item">累计销量{{shopInfo.cSells | sellCountFilter}}</span>
          <span class="sell-item">{{shopInfo.cFans}}人关注</span>
        </div>
      </div>
    </div>

    <div class="score">
      <table>
        <tr v-for="(item, index) in shopInfo.score" :key="index">
          <td>{{item.name}}</td>
          <td>{{item.score}}</td>
          <td>{{item.isBetter ? '高' : '低'}}</td>
        </tr>
      </table>
    </div>

    <div class="blank"></div>
  </div>
</template>

<script>
  export default {
    name: "DetailShopInfo",
    props: {
      shopInfo: Object
    },
    filters: {
      sellCountFilter: function (value) {
        if (value < 10000) return value;
        return (value/10000).toFixed(1) + '万'
      }
    }
  }
</script>

<style scoped>
  .shop-info {
    font-size: 14px;
    color: black;

  }
  .base-info{
    margin-left: 10px;
    display: flex;
    align-items: center;
  }
  .info {
    margin: 10px;
  }
  .sell {
    display: flex;
    align-items: center;
  }
  .sell span {
    margin-right: 4px;
    color: #ff5777;
    border: solid #666666 1px;
    border-radius: 5px;
  }
  .shop-info img {
    width: 90px;
    height: 90px;
    background-color: red;
  }
  .blank {
    height: 10px;
    background-color: rgb(234, 234, 234);
  }
  .score {
    padding-left: 10px;
  }

</style>
