<template>
  <div class="goods-item" @click="itemClick">
    <img :src="showImage" alt="" width=153.6px height=230.4px>
    <div class="goods-info">
      <p>{{goodsItem.title}}</p>
      <span class="price">{{goodsItem.price}}</span>
      <span class="collect">{{goodsItem.cfav}}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "GoodsListItem",
  props: {
    goodsItem: {
      type: Object,
      default() {
        return {}
      }
    },
  },
  // 这个计算属性返回的两个条件互换会出问题，毕竟你不能判断一个没有的东西里面有没有另外一个东西~
  computed: {
    showImage() {
      // return this.product.image || this.product.show.img
      return (this.goodsItem.show && this.goodsItem.show.img) || this.goodsItem.image;
    }
  },
  methods: {
    itemClick() {
      this.$router.push('/detail/' + this.goodsItem.iid)
    }
  }
}
</script>

<style scoped>
  .goods-item {
    padding-bottom: 40px;
    position: relative;
    width: 48%;
  }

  .goods-item img {
    width: 100%;
    border-radius: 5px;
  }

  .goods-info {
    font-size: 12px;
    position: absolute;
    bottom: 5px;
    left: 0;
    right: 0;
    overflow: hidden;
    text-align: center;
  }

  .goods-info p {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    margin-bottom: 3px;
  }

  .goods-info .price {
    color: var(--color-high-text);
    margin-right: 20px;
  }

  .goods-info .collect {
    position: relative;
  }

  .goods-info .collect::before {
    content:'';
    position: absolute;
    left: -15px;
    top:  -1px;
    width: 14px;
    height: 14px;
    background: url("~assets/img/common/collect.svg") 0 0/14px 14px;;
  }
</style>
