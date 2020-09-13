<template>
  <div class="home-recommend">
    <div v-for="item in recommends" class="recommend-item">
      <a :href="item.link">
        <img :src="item.image" alt="">
        <div>{{item.title}}</div>
      </a>
    </div>
  </div>
</template>

<script>
export default {
  name: "HomeRecommend",
  props: {
    recommends: {
      type: Array,
      default() {
        return []
      }
    }
  }
}
</script>

<style scoped>
  .home-recommend {
    display: flex;
  }
  .recommend-item {
    flex: 1;
    text-align: center;
    font-size: 14px;
  }
  img{
    width: 80%;
  }
</style>
