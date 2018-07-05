<template>
  <div class="nav">
    <router-link class="nav-item" to="/goods">
      点餐
      <i class="line"></i>
    </router-link>
    <router-link class="nav-item" to="/ratings">
      评价
      <i class="line"></i>
    </router-link>
    <router-link class="nav-item" to="/seller">
      商家
      <i class="line"></i>
    </router-link>
  </div>
</template>

<script>
  export default {

  }
</script>

<style scoped>
  .nav{
    display: flex;
    width: 100%;
    line-height: 0.8rem;
    background: #fff;
    border-bottom: 1px solid #e4e4e4;
  }
  .nav .nav-item{
    position: relative;
    flex: 1;
    text-align: center;
    font-size: 14px;
    color: #666;
  }
  .nav .active{
    color: #ffbb22;
  }
  .nav .active .line{
    position: absolute;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
    width: 0.4rem;
    height: 0.04rem;
    background: #ffbb22;
  }
</style>
