<template>
  <div class="goods">
    <!-- 分类列表 start -->
    <div class="menu-wrapper">
      <ul>
        <!-- 专场分类 start -->
        <li class="menu-item">
          <p class="text">
            <img class="icon" :src="container.tag_icon" >
            {{container.tag_name}}
          </p>
        </li>
        <!-- 专场分类 end -->

        <!-- 其他分类 start -->
        <li class="menu-item" v-for="(item, index) in goods" :key="index">
          <p class="text">
            <img class="icon" :src="item.icon">
            {{item.name}}
          </p>
        </li>
        <!-- 其他分类 end -->
      </ul>
    </div>
    <!-- 分类列表 end -->

    <!-- 商品列表 start -->
    <div class="foods-wrapper">
      <!-- 专场分类的商品 start -->
      <li>
        <div v-for="(item, index) in container.operation_source_list" :key="index">
          <img :src="item.pic_url">
        </div>
      </li>
      <!-- 专场分类的商品 end -->

      <!-- 其他分类的商品 start -->
      <li class="food-list" v-for="(item, index) in goods" :key="index">
        <h3>{{item.name}}</h3>
        <ul>
          <li class="food-item" v-for="(food, index) in item.spus" :key="index">
            <img :src="food.picture">
            <div class="content">
              <h3 class="name">{{food.name}}</h3>
              <p class="desc">{{food.description}}</p>
              <p class="detail">
                <span class="saled">{{food.month_saled_content}}</span>
                <span class="praised">{{food.praise_content}}</span>
              </p>
              <img class="recommend-icon" :src="food.product_label_picture">
              <p class="price">
                <span class="money">{{food.min_price}}</span>
                <span class="unit">/{{food.unit}}</span>
              </p>
            </div>
          </li>
        </ul>
      </li>
      <!-- 其他分类的商品 end -->
    </div>
    <!-- 商品列表 end -->
  </div>
</template>

<script>
  export default {
    data(){
      return {
        container: {},
        goods: [],
        poiInfo: {}
      }
    },
    created(){
      fetch('/api/goods')
        .then(res => {
          return res.json()
        })
        .then(response => {
          if(response.code === 0){
            this.container = response.data.container_operation_source
            this.goods = response.data.food_spu_tags
            this.poiInfo = response.data.poi_info

          }
        })
    }
  }
</script>

<style scoped>

</style>
