<template>
  <div>
    <div class="img">
      <img :src="itemInfo.imgURL" alt="">
    </div>
  </div>
</template>

<script>
  export default {
    name: "CartListItem",
    props: {
      itemInfo: Object
    }
  }
</script>

<style scoped>
  .img {
    height: 150px;
  }
  .img img{
    height: 100%;
  }
</style>
