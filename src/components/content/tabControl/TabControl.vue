<template>
  <div class="tab-control">
    <!--只有文字不同的话，可以不用插槽，titles一个数组，从父组件中传入-->
    <div v-for="(item, index) in titles"
         class="tab-control-item"
         :class="{active: currentIndex === index}"
         @click="tabControlItemClick(index)">
      <!--点击切换样式，字体颜色改变，加上下划线-->
      <span>{{item}}</span>
    </div>

  </div>
</template>

<script>
  export default {
    name: "TabControl",
    props: {
      titles: {
        type: Array,
        default() {
          return []
        }
      }
    },
    data() {
      return {
        currentIndex: 0
      }
    },
    methods: {
      tabControlItemClick(index) {
        this.currentIndex = index
        this.$emit('tabClick', index)
      }
    }
  }
</script>

<style scoped>
  .tab-control {
    display: flex;
    text-align: center;
    font-size: 17px;
    line-height: 40px;
    height: 40px;
    background-color: #fff;
    /*一开始被覆盖在tabbar下面，为了能够看到tabControl设置了margin-bottom*/
    /*margin-bottom: 50px;*/
  }
  .tab-control-item {
    flex: 1;
  }
  .tab-control-item span {
    padding: 5px;
  }
  .active {
    color: var(--color-high-text);
  }
  .active span {
    border-bottom: solid #42b983 3px;
  }
</style>
