<template>
  <div class="header" :style="header_bg_url">
    <!-- 顶部通栏 -->
    <div class="top-nav">
      <div class="back-btn">
        <span class="icon-arrow_lift"></span>
      </div>
      <div class="search-box">
        <span class="search-ico"></span>
        <input type="text" placeholder="搜索店内商品" />
      </div>
      <div class="more-operate">
        <a class="spell-btn" href="javascript:void(0)">拼单</a>
        <div class="more-btn">
          <span class="circle-ico"></span>
          <span class="circle-ico"></span>
          <span class="circle-ico"></span>
        </div>
      </div>
    </div>
    <!-- 顶部通栏 -->
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
  .header .top-nav{
    line-height: 1rem;
    font-size: 0;
  }
  .top-nav .back-btn, .top-nav .search-box, .top-nav .more-operate{
    display: inline-block;
    font-size: 12px;
  }
  .top-nav .back-btn{
    width: 1rem;
    text-align: center;
  }
  .top-nav .search-box{
    width: 5rem;
  }
</style>
