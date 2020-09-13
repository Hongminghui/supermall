<template>
  <div class="goods-list-item" @click="itemClick">
    <img :src="goodsItem.show.img" alt="">
    <div class="goods-info">
      <p>{{goodsItem.title}}</p>
      <span class="price">{{goodsItem.price}}</span>
      <span class="collect">{{goodsItem.cfav}}</span>
    </div>
  </div>
</template>

<script>
  export default {
    name: "GoodsListItem",
    props: {
      goodsItem: {
        type: Object,
        default() {
          return {}
        }
      }
    },
    methods: {
      itemClick() {
        //记得左右都加杠
        this.$router.push('/detail/' + this.goodsItem.iid)
      }
    }
  }
</script>

<style scoped>
  .goods-list-item {
    width: 48%;
    padding-bottom: 40px;

    position: relative;
  }
  .goods-list-item img{
    width: 100%;
    height: 95%;
    /*给div元素设置圆角*/
    border-radius: 5px;
  }
  .goods-info {
    font-size: 12px;

    position: absolute;
    left: 0;
    right: 0;
    bottom: 5px;

    text-align: center;
  }
  .goods-info p {
    height: 12px;
    overflow: hidden;
    /*显示省略符号来代表被修剪的文本*/
    text-overflow: ellipsis;
    /*段落中的文本不换行*/
    white-space: nowrap;
    margin-bottom: 5px;
  }
  .goods-info .price {
    margin-right: 20px;
  }
  .goods-info .collect {
    position: relative;
  }
  .goods-info .collect::before {
    content: '';
    position: absolute;
    left: -15px;
    top: -1px;
    width: 14px;
    height: 14px;
    background: url("~assets/img/common/collect.svg") 0 0/14px 14px;
  }
</style>
