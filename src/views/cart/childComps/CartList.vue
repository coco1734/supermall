<template>
  <div class="cart-list">
    <scroll class="content" ref="scroll">
      <cart-list-item
        v-for="(item,index) in cartList"
        :key="index"
        :item-info="item">
      </cart-list-item>
    </scroll>
  </div>
</template>

<script>
  import Scroll from "@/components/common/scroll/Scroll";
  import CartListItem from "@/views/cart/childComps/CartListItem";

  import { mapGetters } from "vuex";

  export default {
    name: "CartList",
    components: {
      Scroll,
      CartListItem
    },
    computed: {
      ...mapGetters(['cartList'])
    },
    activated() {
      this.$refs.scroll.refresh()
    }
  }
</script>
<style scoped>
  .cart-list {
    height: 100vh;
  }

  .content {
    height: calc(100vh - 49px - 44px - 40px);
    overflow: hidden;
  }
  /*.cart-list {*/
  /*  height: calc(100%- 44px - 49px - 40px);*/
  /*}*/

  /*.content {*/
  /*  height: 100%;*/
  /*  overflow: hidden;*/
  /*}*/
</style>
