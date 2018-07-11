<template>
  <div class="cart-control">
    <transition name="move">
      <span
        class="decrease-cart"
        @click.stop.prevent="decreaseCart"
        v-show="food.count"
      ></span>
    </transition>
    <span class="cart-num" v-show="food.count">{{food.count}}</span>
    <span
      class="increase-cart"
      @click.stop.prevent="increaseCart"
    ></span>
  </div>
</template>

<script>
  export default {
    data(){
      return {}
    },
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      decreaseCart(){
        this.food.count--
      },
      increaseCart(){
        if(!this.food.count){
          this.$set(this.food, 'count', 1)
        }else{
          this.food.count++
        }
      }
    }
  }
</script>

<style scoped>
  .cart-control{
    display: flex;
    align-items: center;
    position: absolute;
    right: 0;
    bottom: 0.3rem;
    height: 0.3rem;
    font-size: 0;
  }
  .cart-control .decrease-cart, .cart-control .increase-cart{
    position: relative;
    width: 0.3rem;
    height: 0.3rem;
    border-radius: 50%;
    text-align: center;
    font-size: 12px;
  }
  .cart-control .decrease-cart{
    background: #999;
  }
  .cart-control .increase-cart{
    background: #ffc951;
  }
  .cart-control .decrease-cart:before{
    content: '';
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 0.15rem;
    height: 2px;
    background: #fff;
  }
  .cart-control .increase-cart:before, .cart-control .increase-cart:after{
    content: '';
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 0.15rem;
    height: 2px;
    background: #000;
  }
  .cart-control .increase-cart:after{
    transform: translate(-50%, -50%) rotate(90deg);
  }
  .cart-control .cart-num{
    margin: 0 0.15rem;
  }
  .move-enter-active,.move-leave-active{
    transition: all 0.5s linear;
  }
  .move-enter,.move-leave-to{
    transform: translateX(0.3rem) rotate(180deg);
  }
</style>
