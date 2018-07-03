<template>
  <div class="header" :style="header_bg_url">
    <!-- 顶部通栏 start -->
    <div class="top-nav">
      <div class="back-btn">
        <span class="icon-arrow_lift"></span>
      </div>
      <div class="search-box">
        <input type="text" placeholder="搜索店内商品" />
      </div>
      <div class="more-operate">
        <a class="spell-btn" href="javascript:void(0)">拼单</a>
        <span class="more-btn">
          <i class="circle-ico"></i>
          <i class="circle-ico"></i>
          <i class="circle-ico"></i>
        </span>
      </div>
    </div>
    <!-- 顶部通栏 end -->

    <!-- 商家信息 start -->
    <div class="business-info">
      <img class="pic" :src="poiInfo.pic_url" alt="">
      <div class="name">{{poiInfo.name}}</div>
      <div class="favor">收藏</div>
    </div>

    <!-- 商家信息 end -->

    <!-- 优惠活动 start -->
    <div class="discount-activity">
      <img class="icon" v-if="poiInfo.discounts2" :src="poiInfo.discounts2[0].icon_url">
      <span class="text" v-if="poiInfo.discounts2">{{poiInfo.discounts2[0].info}}</span>
      <a class="activity-icon" v-if="poiInfo.discounts2" @click="showMask">
        {{poiInfo.discounts2.length}}个活动
        <span class="icon-keyboard_arrow_right"></span>
      </a>
    </div>
    <!-- 优惠活动 end -->

    <!-- 优惠活动弹窗 start -->
    <div class="activity-mask" v-show="isShowMask">
      <div class="wrapper">
        <div class="content" :style="mask_bg_url">
          <img class="icon" :src="poiInfo.pic_url">
          <h3 class="name">{{poiInfo.name}}</h3>
          <div class="score">
            <app-star :score="poiInfo.wm_poi_score"></app-star>
            <span>{{poiInfo.wm_poi_score}}</span>
          </div>
          <p class="tips">
            {{poiInfo.min_price_tip}} <i>|</i> {{poiInfo.shipping_fee_tip}} <i>|</i> {{poiInfo.delivery_time_tip}}
          </p>
          <p class="time">配送时间：{{poiInfo.shipping_time}}</p>
          <div class="discounts">
            <p>
              <img v-if="poiInfo.discounts2" :src="poiInfo.discounts2[0].icon_url">
              <span v-if="poiInfo.discounts2">{{poiInfo.discounts2[0].info}}</span>
            </p>
          </div>
        </div>
        <a href="javascript:;" class="u-close" @click="closeMask"></a>
      </div>
    </div>
    <!-- 优惠活动弹窗 end -->
  </div>
</template>

<script>
  import Star from '../star/Star'
  export default {
    data(){
      return {
        isShowMask: false
      }
    },
    components: {
      'app-star': Star
    },
    props: {
      poiInfo: {
        type: Object,
        default: {}
      }
    },
    computed: {
      header_bg_url(){
        return "background-image: url(" + this.poiInfo.head_pic_url + ")"
      },
      mask_bg_url(){
        return "background-image: url(" + this.poiInfo.poi_back_pic_url + ")"
      }
    },
    methods:{
      showMask(){
        this.isShowMask = true
      },
      closeMask(){
        this.isShowMask = false
      }
    }
  }
</script>

<style scoped>
  @import url("../../common/css/icon.css");

  .discount-activity {
    background-repeat: no-repeat;
    background-size: 0.5rem 0.5rem;
  }

  .header{
    height: 2.6rem;
    background-size: 100% auto;
    background-position: 0 -0.5rem;
  }
  .header .top-nav{
    display: flex;
    align-items: center;
    line-height: 1rem;
  }
  .top-nav .back-btn{
    width: 1rem;
    text-align: center;
  }
  .top-nav .search-box{
    flex: 1;
    position: relative;
    width: 5rem;
    height: 0.7rem;
    background: #c8c5c9;
    border-radius: 25px;
  }
  .top-nav .search-box:before{
    content: '';
    position: absolute;
    top: 50%;
    left: 0.1rem;
    transform: translateY(-50%);
    width: 0.3rem;
    height: 0.3rem;
    background: url("./img/search.png") no-repeat;
    background-size: 0.3rem 0.3rem;
  }
  .top-nav .search-box input{
    padding-left: 0.5rem;
    line-height: 0.7rem;
    vertical-align: top;
    background: none;
    border: none;
  }
  .top-nav .more-operate{
    width: 1.5rem;
    text-align: right;
  }
  .top-nav .more-operate .spell-btn{
    padding: 0.05rem;
    border: 1px solid #fff;
    text-decoration: none;
    color: #fff;
  }
  .top-nav .more-operate .more-btn{
    margin: 0 0.1rem;
    font-size: 0;
  }
  .top-nav .more-operate .more-btn .circle-ico{
    display: inline-block;
    width: 0.05rem;
    height: 0.05rem;
    margin-right: 0.03rem;
    border: 1px solid #fff;
    border-radius: 50%;
  }
  .business-info, .discount-activity{
    margin: 0 0.2rem;
  }
  .business-info{
    display: flex;
  }
  .business-info .pic{
    width: 1rem;
    height: 1rem;
    margin-right: 0.15rem;
    border-radius: 5px;
  }
  .business-info .name{
    flex: 1;
    line-height: 1rem;
    font-size: 16px;
    color: #fff;
  }
  .business-info .favor{
    width: 0.8rem;
    text-align: right;
    color: #fff;
  }
  .business-info .favor:before{
    content: '';
    display: block;
    width: 100%;
    height: 0.4rem;
    margin: 0.1rem 0;
    background: url("./img/star.png") no-repeat top right;
    background-size: 0.4rem 0.4rem;
  }
  .discount-activity{
    display: flex;
    align-items: center;
    margin-top: 0.1rem;
  }
  .discount-activity .icon{
    width: 0.3rem;
    height: 0.3rem;
    margin-right: 0.1rem;
    border-radius: 2px;
  }
  .discount-activity .text{
    flex: 1;
    color: #fff;
  }
  .discount-activity .activity-icon{
    width: 1.1rem;
    text-align: right;
    color: #fff;
  }
  .activity-mask{
    position: absolute;
    top: 0;
    left: 0;
    z-index: 99;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
  }
  .activity-mask h3, .activity-mask p, .activity-mask span{
    color: #fff;
  }
  .activity-mask .wrapper{
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    text-align: center;
  }
  .activity-mask .content{
    width: 6.5rem;
    height: 12rem;
    background-size: 7rem 12rem;
    border-radius: 10px;
  }
  .activity-mask .content .icon{
    width: 1.5rem;
    height: 1.5rem;
    margin-top: 0.8rem;
    border-radius: 5px;
  }
  .activity-mask .content h3{
    margin: 0.25rem 0 0.1rem;
    font-size: 14px;
    font-weight: normal;
  }
  .activity-mask .content p{
    line-height: 2;
    color: #bababc;
  }
  .activity-mask .content .tips i{
    margin: 0 0.14rem;
    font-style: normal;
    color: #bababc;
  }
  .activity-mask .content .discounts{
    width: 90%;
    margin: 0.2rem auto;
    padding-top: 0.2rem;
    border-top: 1px solid #bababc;
  }
  .activity-mask .content .discounts img{
    width: 0.3rem;
    height: 0.3rem;
    margin-right: 0.1rem;
    vertical-align: text-bottom;
    border-radius: 2px;
  }
  .activity-mask .u-close{
    position: relative;
    display: inline-block;
    width: 0.8rem;
    height: 0.8rem;
    margin-top: 0.3rem;
    background: rgba(118, 118, 118, 0.8);
    border-radius: 50%;
    transform: rotate(45deg);
  }
  .activity-mask .u-close:before, .activity-mask .u-close:after{
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    content: '';
    width: 1px;
    height: 15px;
    background: #fff;
  }
  .activity-mask .u-close:before{
    transform: translate(-50%, -50%) rotate(90deg);
    transform-origin: 50% 50%;
  }




</style>
