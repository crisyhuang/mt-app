<template>
  <div class="product-detail" v-show="flag">
    <!-- 外卖详情 start -->
    <div class="food-content">
      <img class="food-pic" :src="food.picture" >
      <div class="tool-icons">
        <span class="close-icon" @click="hideProduct"></span>
        <span class="share-icon">
          <img src="./img/share.png" >
        </span>
        <span class="more-icon">
          <img src="./img/more.png" >
        </span>
      </div>
      <div class="food-desc-wrapper">
        <div class="food-desc">
          <h3>{{food.name}}</h3>
          <p>{{food.month_saled_content}}</p>
          <p><b>&yen; {{food.min_price}}</b>/{{food.unit}}</p>
        </div>
        <div class="select-box">
          <div class="cart-control-wrapper" v-show="food.count">
            <app-cart-control :food="food"></app-cart-control>
          </div>
          <div class="select-spec" v-show="!food.count || food.count === 0" @click="addCart">选规格</div>
        </div>
      </div>
    </div>
    <!-- 外卖详情 end -->

    <!-- 外卖评价 start -->
    <div class="food-ratings">
      <div class="hd">
        <div class="hd-left" v-if="food.rating">
          <span class="rating-title">{{food.rating.title}}</span>
          <span class="like-desc">({{food.rating.like_ratio_desc}} <b>{{food.rating.like_ratio}}</b>)</span>
        </div>
        <div class="hd-right" v-if="food.rating">
          <span class="comment-count">{{food.rating.comment_count}}条评论</span>
          <span class="icon-keyboard_arrow_right"></span>
        </div>
      </div>
      <div class="bd">
        <ul class="comment-list" v-if="food.rating">
          <li class="comment-item" v-for="(comment, index) in food.rating.comment_list" :key="index">
            <img :src="comment.user_icon" v-if="comment.user_icon">
            <img src="./img/anonymity.png" v-if="!comment.user_icon">
            <div class="comment-detail">
              <p class="user-name">{{comment.user_name}}</p>
              <p class="comment-content">{{comment.comment_content}}</p>
              <p class="comment-time">{{comment.comment_time}}</p>
            </div>
          </li>
        </ul>
      </div>
    </div>
    <!-- 外卖评价 end -->
  </div>
</template>

<script>
  import Cartcontrol from '../cartcontrol/Cartcontrol'

  export default {
    components: {
      'app-cart-control': Cartcontrol
    },
    data(){
      return{
        flag: false
      }
    },
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      showProduct(){
        this.flag = true
      },
      hideProduct(){
        this.flag = false
      },
      addCart(){
        this.$set(this.food, 'count', 1)
      }
    }
  }
</script>

<style scoped>
  .product-detail{
    position: fixed;
    top: 0;
    left: 0;
    bottom: 1rem;
    z-index: 90;
    width: 100%;
    background: #fff;
  }
  .product-detail .food-content{
    padding: 0.2rem;
  }
  .product-detail .food-content .food-pic{
    width: 100%;
  }
  .product-detail .food-content .tool-icons{
    display: flex;
    align-items: center;
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    line-height: 0.8rem;
  }
  .product-detail .food-content .tool-icons .close-icon{
    flex: 0 0 0.6rem;
    position: relative;
    width: 0.6rem;
    height: 0.6rem;
    margin-left: 0.15rem;
    background: #767276;
    border-radius: 50%;
    transform: rotate(45deg);
  }
  .product-detail .food-content .tool-icons .close-icon:before,
  .product-detail .food-content .tool-icons .close-icon:after{
    content: '';
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 70%;
    height: 2px;
    background: #fff;
  }
  .product-detail .food-content .tool-icons .close-icon:before{
    transform: translate(-50%, -50%) rotate(90deg);
  }
  .product-detail .food-content .tool-icons img{
    width: 0.6rem;
    height: 0.6rem;
    margin-right: 0.15rem;
    vertical-align: middle;
  }
  .product-detail .food-content .tool-icons .share-icon{
    flex: 1;
    text-align: right;
  }
  .product-detail .food-content .tool-icons .more-icon{
    flex: 0 0 0.6rem;
  }
  .product-detail .food-content .food-desc-wrapper{
    display: flex;
  }
  .product-detail .food-content .food-desc-wrapper .food-desc{
    flex: 1;
  }
  .product-detail .food-content .food-desc-wrapper .select-box{
    flex: 0 0 2rem;
    align-self: flex-end;
    position: relative;
  }
  .product-detail .food-content .select-box .select-spec{
    position: absolute;
    right: 0;
    bottom: 0;
    width: 1rem;
    line-height: 0.5rem;
    background: #ffc951;
    border-radius: 15px;
    text-align: center;
    font-weight: bold;
  }
  .product-detail .food-content .select-box .cart-control{
    bottom: 0;
  }
  .product-detail .food-content .food-desc{
    line-height: 0.35rem;
  }
  .product-detail .food-content .food-desc h3{
    font-size: 16px;
  }
  .product-detail .food-content .food-desc p{
    color: #bfbfbf;
  }
  .product-detail .food-content .food-desc p b{
    color: #fb4e44;
  }
  .product-detail .food-ratings{
    padding: 0.2rem;
    border-top: 0.14rem solid #f3f2f3;
  }
  .product-detail .food-ratings .hd{
    display: flex;
  }
  .product-detail .food-ratings .hd-left{
    flex: 0 0 3rem;
  }
  .product-detail .food-ratings .hd-left .like-desc b{
    color: #fb4e44;
  }
  .product-detail .food-ratings .hd-right{
    flex: 1;
    text-align: right;
  }
  .product-detail .food-ratings .hd-right span{
    color: #999;
  }
  .product-detail .food-ratings .hd-right .icon-keyboard_arrow_right{
    display: inline-block;
    width: 0.1rem;
    height: 0.1rem;
    color: #333;
  }
  .product-detail .food-ratings .comment-item{
    display: flex;
    padding: 0.2rem 0;
    border-bottom: 1px solid #ececec;
  }
  .product-detail .food-ratings .comment-item:last-child{
    border-bottom: none;
  }
  .product-detail .food-ratings .comment-item img{
    flex: 0 0 0.6rem;
    width: 0.6rem;
    height: 0.6rem;
    margin-right: 0.15rem;
    border-radius: 50%;
  }
  .product-detail .food-ratings .comment-item .comment-detail{
    flex: 1;
    position: relative;
  }
  .product-detail .comment-item .comment-detail .comment-content{
    line-height: 0.3rem;
    padding-top: 0.1rem;
  }
  .product-detail .comment-item .comment-detail .comment-time{
    position: absolute;
    right: 0;
    top: 0;
  }



</style>
