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
      <div class="pic">
        <img :src="poiInfo.pic_url" alt="">
      </div>
      <div class="name">{{poiInfo.name}}</div>
      <div class="favor">收藏</div>
    </div>

    <!-- 商家信息 end -->

    <!-- 优惠活动 start -->
    <div class="discount-activity">
      <img class="icon" v-if="poiInfo.discounts2" :src="poiInfo.discounts2[0].icon_url">
      <span class="text" v-if="poiInfo.discounts2">{{poiInfo.discounts2[0].info}}</span>
      <div class="detail" v-if="poiInfo.discounts2">
        {{poiInfo.discounts2[0].length}}个活动
      </div>
    </div>

    <!-- 优惠活动 end -->
  </div>
</template>

<script>
  export default {
    data(){
      return {}
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
    height: 2.5rem;
    background-size: 100% auto;
    background-position: 0 -0.5rem;
  }
  .header .top-nav{
    line-height: 1rem;
    font-size: 0;
  }
  .top-nav .back-btn, .top-nav .search-box, .top-nav .more-operate{
    display: inline-block;
    vertical-align: middle;
    font-size: 12px;
  }
  .top-nav .back-btn{
    width: 1rem;
    text-align: center;
    color: #fff;
  }
  .top-nav .search-box{
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
    width: 4.5rem;
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
  .business-info{
    display: flex;
  }
  .business-info .pic{
    width: 1rem;
    height: 1rem;
    margin: 0 0.15rem 0 0.2rem;
  }
  .business-info .pic img{
    width: 100%;
    height: 100%;
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
    text-align: center;
    color: #fff;
  }
  .business-info .favor:before{
    content: '';
    display: inline-block;
    width: 100%;
    height: 0.4rem;
    margin: 0.1rem 0 0.05rem;
    background: url("./img/star.png") no-repeat top center;
    background-size: 0.4rem 0.4rem;
  }

</style>
