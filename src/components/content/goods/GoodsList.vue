<template>
  <div class="goods-list">
    <goods-list-item v-for="item in goods" :goods-item="item"></goods-list-item>
  </div>
</template>

<script>
  import GoodsListItem from "./GoodsListItem";
  export default {
    name: "GoodsList",
    components: {
      GoodsListItem
    },
    props: {
      goods: {
        type: Array,
        default() {
          return []
        }
      }
    }
  }
</script>

<style scoped>
  .goods-list {
    display: flex;
    /*弹性盒换行*/
    flex-wrap: wrap;
    /*弹性盒内容两侧留下空白*/
    justify-content: space-around;
  }
</style>
