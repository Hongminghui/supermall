<template>
  <div>
    <nav-bar><div slot="center">购物车({{$store.getters.cartCount}})</div></nav-bar>
    <cart-list :cart-list="cartList"></cart-list>
  </div>
</template>

<script>
  import NavBar from "../../components/common/navbar/NavBar";
  import CartList from "./childComps/CartList";
  export default {
    name: "Cart",
    computed: {
      cartList() {
        return this.$store.getters.cartList
      }
    },
    components: {
      NavBar,
      CartList
    }
  }
</script>

<style scoped>
  .navbar {
    background-color: #ff5777;
  }
</style>
