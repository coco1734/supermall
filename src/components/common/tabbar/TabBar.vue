<template>
  <div id="tab-bar">
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: "TabBar"
}
</script>

<style scoped>
  /* 实现水平分布 */
  #tab-bar {
    display:  flex;
    /* 添加背景颜色 */
    background-color: #f6f6f6 ;

    /* 将文字设置到页面底部 */
    position: fixed;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: 8;

    /* 界面和文字过渡部分添加阴影 */
    box-shadow: 0 -3px 1px rgba(100,100,100,.1);
  }


</style>
