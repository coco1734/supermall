<template>
  <div class="bottom-bar">
    <div class="check-content">
      <check-button
        :is-checked="isSelectAll"
        class="check-button"
        @click.native="checkClick">
      </check-button>
      <span>全选</span>
    </div>

    <div class="price">
      合计: {{totalPrice}}
    </div>

    <div class="calculate" @click="calcClick">
      去结算 ({{checkLength}})
    </div>
  </div>
</template>

<script>
  import CheckButton from "@/components/content/checkButton/CheckButton";

  export default {
    name: "CartBottomBar",
    components: {
      CheckButton
    },
    computed: {
      totalPrice() {
        return '￥' + this.$store.state.cartList.filter(item => {
          return item.checked
        }).reduce((preValue, item) => {
          return preValue + item.price * item.count
        },0).toFixed(2)
      },
      checkLength() {
        return this.$store.state.cartList.filter(item => item.checked).length
      },
      isSelectAll() {

        if(this.$store.state.cartList.length === 0) return false

        // return  !this.$store.state.cartList.filter(item => !item.checked).length //有bug

        // return  this.$store.state.cartList.every(item => item.checked)

        return  !this.$store.state.cartList.find(item => !item.checked)

        // for (let item of this.cartList) {
        //   if (!item.checked) {
        //     return false
        //   }
        // }
        //
        // return true
      }
    },
    methods: {
      checkClick() {
        // console.log('---');

        //方案1
        // let checked =!this.isSelectAll;
        // this.cartList.forEach(p => p.checked = checked);

        //方案2
        if (this.isSelectAll) {//全选
          this.cartList.forEach(item => item.checked = false)
        } else {  //部分或全部不选
          this.cartList.forEach(item => item.checked = true)
        }
      },
      calcClick() {
        if (!this.isSelectAll) {
          this.$toast.show('请选择购买的商品', 2000)
        }
      }
    }
  }
</script>

<style scoped>
  .bottom-bar {
    position: relative;
    background-color: #eee;
    display: flex;
    bottom: 135px;
    line-height: 40px;
    height: 40px;
  }

  .check-content {
    display: flex;
    line-height: 40px;
    align-items: center;
    margin-left: 10px;

  }

  .check-button {
    width: 20px;
    height: 20px;
    line-height: 20px;
    margin-right: 5px;
  }

  .price {
    margin-left: 40px;
  }

  .calculate {
    position: absolute;
    right: 0px;
    width: 80px;
    background-color: red;
    text-align: center;

  }
</style>
