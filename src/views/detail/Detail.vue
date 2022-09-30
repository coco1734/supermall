<template>
  <div id="detail">
    <detail-nav-bar class="detail-nav" @titleClick="titleClick" ref="nav"></detail-nav-bar>
    <scroll class="content"
            ref="scroll"
            :probe-type="3"
            @scroll="contentScroll">
<!--      <ul>-->
<!--        <li v-for="item in $store.state.cartList">-->
<!--          {{item}}-->
<!--        </li>-->
<!--      </ul>-->
<!--      <div>{{$store.state.cartList.length}}</div>-->
      <detail-swiper :top-images="topImages"></detail-swiper>
      <detail-base-info :goods="goods"></detail-base-info>
      <detail-shop-info :shop="shop"></detail-shop-info>
      <detail-goods-info :detail-info="detailInfo" @imageLoad="imageLoad"></detail-goods-info>
      <detail-param-info ref="params" :param-info="paramInfo"></detail-param-info>
      <detail-comment-info ref="comment" :comment-info="commentInfo"></detail-comment-info>
      <goods-list ref="recommend" :goods="recommends"></goods-list>
    </scroll>
    <detail-bottom-bar @addCart="addToCart"></detail-bottom-bar>
    <back-top @click.native="backClick" v-show="isShowBackTop"></back-top>
<!--    <toast :message="message" :show="show"></toast>-->
  </div>
</template>

<script>
  import DetailNavBar from "@/views/detail/childComps/DetailNavBar";
  import DetailSwiper from "@/views/detail/childComps/DetailSwiper";
  import DetailBaseInfo from "@/views/detail/childComps/DetailBaseInfo";
  import DetailShopInfo from "@/views/detail/childComps/DetailShopInfo";
  import DetailGoodsInfo from "@/views/detail/childComps/DetailGoodsInfo";
  import DetailParamInfo from "@/views/detail/childComps/DetailParamInfo";
  import DetailCommentInfo from "@/views/detail/childComps/DetailCommentInfo";
  import DetailBottomBar from "@/views/detail/childComps/DetailBottomBar";
  // import Toast from "@/components/common/toast/Toast";

  import Scroll from "@/components/common/scroll/Scroll";
  import GoodsList from "@/components/content/goods/GoodsList";
  import BackTop from "@/components/content/backTop/BackTop";


  import {getDetail, Goods, Shop, GoodsParam,getRecommend} from "@/network/detail";

  // import {mapActions} from "vuex";

  export default {
    name: "Detail",
    components: {
      DetailNavBar,
      DetailSwiper,
      DetailBaseInfo,
      DetailShopInfo,
      DetailGoodsInfo,
      DetailParamInfo,
      DetailCommentInfo,
      DetailBottomBar,
      Scroll,
      GoodsList,
      BackTop,
      // Toast
    },
    data() {
      return {
        iid: null,
        topImages: [],
        goods: {},
        shop: {},
        detailInfo: {},
        paramInfo: {},
        commentInfo: {},
        recommends: [],
        themeTopYs: [],
        currentIndex: 0,
        isShowBackTop: false,
        // message: '',
        // show: false
      }
    },
    created() {
      //1.保存传入的iid
      this.iid = this.$route.params.iid

      //2.根据iid请求详情数据
      getDetail(this.iid).then(res => {

        console.log(res);

        const data = res.result;

        // 1.获取顶部的图片轮播数据
        this.topImages = data.itemInfo.topImages

        // 2.获取商品信息
        this.goods = new Goods(data.itemInfo, data.columns,data.shopInfo.services)

        //3.创建店铺信息的对象
        this.shop = new Shop(data.shopInfo)

        // 4.保存商品的详情数据
        this.detailInfo = data.detailInfo

        //5.获取参数的信息
        this.paramInfo = new GoodsParam(data.itemParams.info, data.itemParams.rule)

        //7.取出评论的信息
        if(data.rate.cRate !== 0) {
          this.commentInfo = data.rate.list[0]
        }
      })

      //3.请求推荐数据
      getRecommend().then(res => {
        // console.log(res);
        this.recommends = res.data.list
      })
    },
    methods: {
      // ...mapActions(['addCart']),
      imageLoad() {
        this.$refs.scroll.refresh()

        this.themeTopYs = []

        this.themeTopYs.push(0);
        this.themeTopYs.push(this.$refs.params.$el.offsetTop)
        this.themeTopYs.push(this.$refs.comment.$el.offsetTop)
        this.themeTopYs.push(this.$refs.recommend.$el.offsetTop)
        this.themeTopYs.push(Number.MAX_VALUE)

        // console.log(this.themeTopYs);
      },
      titleClick(index) {
        // console.log(index);
        this.$refs.scroll.scrollTo(0, -this.themeTopYs[index],200)
      },
      contentScroll(position) {
        // console.log(position);
        //1获取y值
        const  positionY = -position.y

        //2positionY和主题中值进行对比
        let length = this.themeTopYs.length
        for(let i = 0; i < length-1; i++) {
          if (this.currentIndex !== i && (positionY >= this.themeTopYs[i] && positionY < this.
            themeTopYs[i+1])){
            this.currentIndex = i;
            this.$refs.nav.currentIndex = this.currentIndex;
            // console.log(this.currentIndex);
          }
        }
        //3.是否显示回到顶部
        this.isShowBackTop = -position.y > 1000

      },
      backClick() {
        this.$refs.scroll.scrollTo(0,0,300)
      },
      addToCart() {
        // console.log('---');
        //1.获取购物车需要展示的信息
        const product = {}
        product.image = this.topImages[0]
        product.title = this.goods.title
        product.desc = this.goods.desc
        product.price = this.goods.realPrice
        product.iid = this.iid

        //2.将商品添加到购物车里(1.Promise 2.mapActions)
        // this.$store.cartList.push(product)
        // this.$store.commit('addCart', product)

        // this.addToCart(product).then(res => {
        //   console.log(res);
        // })

        this.$store.dispatch('addCart',product).then(res => {
          console.log(res);
          // this.show = true;
          // this.message = res;
          //
          // setTimeout(() => {
          //   this.show = false;
          //   this.message = ''
          // },1500)

          this.$toast.show(res, 2000)
        })

      }
    },


    activated() {
      //有缓存功能需要在activated设置

    }
  }
</script>

<style scoped>
  #detail {
    position: relative;
    z-index: 9;
    background-color: #fff;
    height: 100vh;
  }

  .detail-nav {
    position: relative;
    z-index: 9;
    background-color: #fff;
  }

  .content {
    height: calc(100% - 44px - 49px);
  }
</style>
