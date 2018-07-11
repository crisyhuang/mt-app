<template>
  <div class="shopping-cart">
    <div class="shopping-cart-wrapper">
      <!-- 底部左侧 start -->
      <div class="content-left">
        <div class="icon-cart-wrapper" :class="{highlight: totalCount > 0}" @click="toggleList">
          <span class="icon-shopping_cart icon-cart"></span>
          <span class="total-count" v-show="totalCount">{{totalCount}}</span>
        </div>
        <div class="desc-wrapper">
          <p class="total-price" v-show="totalCount">&yen; {{totalPrice}}</p>
          <p class="dispatch-price" :class="{highlight: totalCount > 0}">另需{{poiInfo.shipping_fee_tip}}</p>
        </div>
      </div>
      <!-- 底部左侧 end -->

      <!-- 底部右侧 start -->
      <div class="content-right" :class="{highlight: totalCount > 0}">{{payDesc}}</div>
      <!-- 底部右侧 end -->

      <!-- 购物车列表 start -->
      <div class="shopping-cart-list" v-show="showCartList">
        <div class="cart-list-wrapper">
          <p class="list-top" v-if="poiInfo.discounts2">{{poiInfo.discounts2[0].info}}</p>
          <div class="list-header">
            <p>1号口袋</p>
            <div class="empty-cart" @click="emptyCart">
              <img src="./img/ash_bin.png" >
              <span>清空购物车</span>
            </div>
          </div>
          <div class="list-content" ref="listContent">
            <ul>
              <li class="food-item" v-for="(food, index) in selectedFood" :key="index">
                <div class="desc">
                  <h3>{{food.name}}</h3>
                  <p class="unit">{{food.unit}}</p>
                </div>
                <div class="price">&yen; {{food.min_price}}</div>
                <div class="cart-control-wrapper">
                  <app-cart-control :food="food"></app-cart-control>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </div>
      <!-- 购物车列表 end -->
    </div>
    <div class="cart-mask" v-show="showCartList" @click="hideMask"></div>
  </div>
</template>

<script>
  import BScroll from 'better-scroll'
  import Cartcontrol from '../cartcontrol/Cartcontrol'

  export default {
    components: {
      'app-cart-control': Cartcontrol
    },
    data(){
      return {
        flag: true
      }
    },
    props: {
      poiInfo: {
        type: Object,
        default: {}
      },
      selectedFood: {
        type: Array,
        default: []
      }
    },
    computed: {
      totalCount(){
        let totalCount = 0
        this.selectedFood.forEach((food) => {
          totalCount += food.count
        })
        return totalCount
      },
      totalPrice(){
        let totalPrice = 0
        this.selectedFood.forEach((food) => {
          totalPrice += food.count * food.min_price
        })
        return totalPrice
      },
      payDesc(){
        if(this.totalCount > 0){
          return '去结算'
        }else{
          return this.poiInfo.min_price_tip
        }
      },
      showCartList(){
        if(!this.totalCount){
          this.flag = true
          return false
        }

        let show = !this.flag

        if(show){
          this.$nextTick(() => {
            if(!this.listScroll){
              this.listScroll = new BScroll(this.$refs.listContent, {
                click: true
              })
            }else{
              this.listScroll.refresh()
            }
          })
        }


        return show
      }
    },
    methods: {
      toggleList(){
        if(!this.totalCount){
          return false
        }else{
          this.flag = !this.flag
        }
      },
      hideMask(){
        this.flag = true
      },
      emptyCart(){
        this.selectedFood.forEach((food) => {
          food.count = 0;
        })
      }
    }
  }
</script>

<style scoped>
  @import url('../../common/css/icon.css');

  .shopping-cart-wrapper{
    position: fixed;
    left: 0;
    bottom: 0;
    z-index: 100;
    display: flex;
    width: 100%;
    height: 1rem;
    background: #514f4f;
  }
  .shopping-cart-wrapper span{
    color: #fff;
  }
  .shopping-cart-wrapper .content-left, .shopping-cart-wrapper .content-right{
    position: relative;
    z-index: 100;
  }
  .shopping-cart-wrapper .content-left{
    display: flex;
    flex: 1;
  }
  .shopping-cart-wrapper .icon-cart-wrapper{
    position: relative;
    flex: 0 0 1rem;
    height: 1rem;
    margin-left: 0.2rem;
    margin-top: -0.35rem;
    background: #666;
    border-radius: 50%;
    text-align: center;
  }
  .shopping-cart-wrapper .icon-cart-wrapper .icon-cart{
    line-height: 1rem;
    font-size: 25px;
    color: #c4c4c4;
  }
  .shopping-cart-wrapper .icon-cart-wrapper.highlight{
    background: #ffc95a;
  }
  .shopping-cart-wrapper .icon-cart-wrapper.highlight .icon-cart{
    color: #000;
  }
  .shopping-cart-wrapper .icon-cart-wrapper .total-count{
    position: absolute;
    top: 0;
    right: 0;
    width: 0.3rem;
    height: 0.3rem;
    line-height: 0.28rem;
    border-radius: 50%;
    background: #e90515;
    color: #fff;
  }
  .shopping-cart-wrapper .desc-wrapper{
    flex: 1;
    padding-left: 0.1rem;
  }
  .shopping-cart-wrapper .desc-wrapper .total-price{
    padding: 0.12rem 0 0.05rem;
    font-size: 16px;
    color: #fff;
  }
  .shopping-cart-wrapper .desc-wrapper .dispatch-price{
    line-height: 1rem;
    color: #bab9b9;
  }
  .shopping-cart-wrapper .desc-wrapper .dispatch-price.highlight{
    line-height: 0.4rem;
  }
  .shopping-cart-wrapper .content-right{
    flex: 0 0 2.2rem;
    line-height: 1rem;
    text-align: center;
    font-size: 16px;
    color: #bab9b9;
  }
  .shopping-cart-wrapper .content-right.highlight{
    background: #ffc95a;
    color: #000;
  }
  .shopping-cart-wrapper .shopping-cart-list{
    position: absolute;
    top:0;
    left: 0;
    z-index: 99;
    transform: translateY(-100%);
    width: 100%;
  }
  .shopping-cart-list .list-top{
    line-height: 0.5rem;
    text-align: center;
    background: #f4e1c2;
    color: #333;
  }
  .shopping-cart-list .list-header{
    display: flex;
    line-height: 0.4rem;
    background: #f5f1f5;
    border-left: 2px solid #5cb540;
  }
  .shopping-cart-list .list-header p{
    flex: 1;
    padding-left: 0.1rem;
  }
  .shopping-cart-list .list-header .empty-cart{
    flex: 0 0 1.8rem;
  }
  .shopping-cart-list .list-header .empty-cart img{
    width: 0.3rem;
    height: 0.3rem;
    vertical-align: sub;
  }
  .shopping-cart-list .list-header .empty-cart span{
    color: #000;
  }
  .shopping-cart-list .list-content{
    max-height: 3rem;
    overflow: hidden;
    background: #fff;
  }
  .shopping-cart-list .list-content .food-item{
    display: flex;
    align-items: center;
    height: 1rem;
    border-bottom: 1px solid #f9f8f9;
  }
  .shopping-cart-list .list-content .food-item .desc{
    flex: 1;
    padding-left: 0.2rem;
  }
  .shopping-cart-list .list-content .food-item .desc h3{
    padding-bottom: 0.1rem;
  }
  .shopping-cart-list .list-content .food-item .desc .unit{
    color: #999;
  }
  .shopping-cart-list .list-content .food-item .price{
    flex: 0 0 2rem;
  }
  .shopping-cart-list .list-content .food-item .cart-control-wrapper{
    flex: 0 0 1.5rem;
    position: relative;
  }
  .shopping-cart-list .list-content .food-item .cart-control-wrapper .cart-control{
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 100%;
  }
  .shopping-cart .cart-mask{
    position: fixed;
    top: 0;
    left: 0;
    z-index: 98;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
  }


</style>
