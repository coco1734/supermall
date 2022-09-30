<template>
  <div class="wrapper" ref="wrapper">
    <div class="content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
  import BScroll from "better-scroll";

  export default {
    name: "Scroll",
    props: {
      probeType: {
        type: Number,
        default: 0
      },
      pullUpLoad: {
        type:Boolean,
        default: false
      }
    },
    data() {
      return {
        scroll: null
      }
    },
    mounted() {
      //1.创建BScroll对象
      this.scroll = new BScroll(this.$refs.wrapper,{
        click: true,
        probeType: this.probeType,
        pullUpLoad: this.pullUpLoad,
        observeDom:true,
        observeImage:true
      })

      //2.监听滚动的位置
    if (this.probeType === 2 || this.probeType === 3) {
       this.scroll.on('scroll', (position) => {
         // console.log(position);
         this.$emit('scroll', position)
       })
    }
     // 在scroll中的，mounted中添加observeDOM: true就不需要来回切换手机端和电脑端了
     //3.监听scroll滚动到底部
     if (this.pullUpLoad) {
        this.scroll.on('pullingUp', () => {
          this.$emit('pullingUp')
        })
     }
    },
    methods: {
      scrollTo(x,y,time=300) {
        this.scroll.scrollTo(x,y,time)
      },
      finishPullUp() {
        this.scroll.finishPullUp()
      },
      refresh(reload) {
        this.scroll.refresh()
      }
    }
  }
</script>

<style scoped>

</style>
