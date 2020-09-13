<template>
  <div class="detailNavBar">
    <nav-bar>
      <div slot="left" class="navLeft" @click="backClick">
        <i class="el-icon-arrow-left"></i>
      </div>
      <div slot="center" class="navCenter">
        <div v-for="(item, index) in detailNav" class="navCenterItem"
              :class="{is_active: currentIndex === index}"
              @click="itemClick(index)">{{item}}
        </div>
      </div>
    </nav-bar>
  </div>
</template>

<script>
  import NavBar from "../../../components/common/navbar/NavBar";
  export default {
    name: "DetailNavBar",
    components: {
      NavBar
    },
    data() {
      return {
        detailNav: ['商品', '参数', '评论', '推荐'],
        // currentIndex: 0
      }
    },
    props: {
      currentIndex: Number
    },
    methods: {
      itemClick(index) {
        // this.currentIndex = index
        this.$emit('navItemClick', index)
      },
      backClick() {
        this.$router.go(-1)
      }
    }
  }
</script>

<style scoped>
  .detailNavBar {
    width: 100%;
    position: fixed;
    z-index: 9;
    background-color: white;
  }
  .navLeft {
    color: #1989fa;
    text-align: center;
  }
  .navCenter {
    flex: 1;
    display: flex;
    text-align: center;
  }
  .navCenterItem {
    flex: 1;
  }
  .is_active {
    color: red;
    border-bottom: solid 1px #ff5777;
  }
</style>
