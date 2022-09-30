<template>
  <div class="tab-bar-item" @click="itemClick">
    <div v-if="!isActive"><slot name="item-icon"></slot></div>
    <div v-else><slot name="item-icon-active"></slot></div>
    <div :style="activeStyle"><slot name="item-text"></slot></div>

<!--   <img src="../../assets/img/tabbar/home.svg" alt="">-->
<!--    <div>首页</div>-->
  </div>
</template>

<script>
export default {
  name: "TabBarItem",
  data() {
    return {
      // isActive: true
    }
  },
  computed: {
    isActive() {
      return this.$route.path.indexOf(this.path) !== -1
    },
    activeStyle() {
      return this.isActive ? {color:this.activeColor} : {}
    }
  },
  props: {
    path: String,
    activeColor:{
      type: String,
      default: 'green'
    }
  },
  methods: {
    itemClick() {
      // console.log('itemClick');
      this.$router.replace(this.path)
    }
  }
}
</script>

<style scoped>
  /* 等分分布 给每个分类都设置一个类*/
  .tab-bar-item {
    flex: 1;
    /* 居中 */
    text-align: center;
    /* 设置高度 一般都设置为49*/
    height: 49px;
    font-size: 14px;


  }

  /* 设置图片的大小 */
  .tab-bar-item img{
    width: 24px;
    height: 24px;
    /* 图片距上面有一定空间 */
    margin-top: 3px;
    /* 调整图片和文字中间距离 */
    vertical-align: middle;
    margin-bottom: 2px;
  }

  /*.active {*/
  /*  color: #FF5777;*/
  /*}*/
</style>
