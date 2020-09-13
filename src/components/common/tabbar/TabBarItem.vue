<template>
  <div id="tabBarItem" @click="itemClick">
    <div v-if="isActive"><slot name="item-active-icon"></slot></div>
    <div v-else><slot name="item-icon"></slot></div>
    <div><slot name="item-text"></slot></div>
  </div>
</template>

<script>
  export default {
    name: "TabBarItem",
    props: {
      path: String
    },
    methods: {
      itemClick() {
        this.$router.replace(this.path)
      }
    },
    computed: {
      isActive() {
        return this.$route.path === this.path
      }
    }
  }
</script>

<style scoped>
  #tabBarItem{
    flex: 1;
    text-align: center;
    margin-top: 2px;
  }
</style>
