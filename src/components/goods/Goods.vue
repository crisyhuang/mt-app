<template>
  <div class="goods">
    <!-- 分类列表 start -->
    <div class="menu-wrapper" ref="menuScroll">
      <ul>
        <!-- 专场分类 start -->
        <li class="menu-item"
            :class="{current:currentIndex === 0}"
            @click="selectMenu(0)">
          <p class="text">
            <img class="icon" :src="container.tag_icon" >
            {{container.tag_name}}
          </p>
        </li>
        <!-- 专场分类 end -->

        <!-- 其他分类 start -->
        <li class="menu-item"
            v-for="(item, index) in goods" :key="index"
            :class="{current:currentIndex === index + 1}"
            @click="selectMenu(index+1)">
          <p class="text">
            <img class="icon" v-if="item.icon" :src="item.icon">
            {{item.name}}
            <i v-if="calCount(item.spus)">{{calCount(item.spus)}}</i>
          </p>
        </li>
        <!-- 其他分类 end -->
      </ul>
    </div>
    <!-- 分类列表 end -->

    <!-- 商品列表 start -->
    <div class="foods-wrapper" ref="foodScroll">
      <ul>
        <!-- 专场分类的商品 start -->
        <li class="container-list food-list-hook">
          <div v-for="(item, index) in container.operation_source_list" :key="index">
            <img :src="item.pic_url">
          </div>
        </li>
        <!-- 专场分类的商品 end -->

        <!-- 其他分类的商品 start -->
        <li class="food-list food-list-hook" v-for="(item, index) in goods" :key="index">
          <h3 class="classify-name">{{item.name}}</h3>
          <ul>
            <li class="food-item"
                v-for="(food, index) in item.spus" :key="index"
                @click="showDetail(food)">
              <img :src="food.picture">
              <div class="content">
                <h3 class="name">{{food.name}}</h3>
                <p class="desc" v-if="food.description">{{food.description}}</p>
                <p class="detail">
                  <span class="saled">{{food.month_saled_content}}</span>
                  <span class="praised">{{food.praise_content}}</span>
                </p>
                <img class="recommend-icon" v-if="food.product_label_picture" :src="food.product_label_picture">
                <p class="price">
                  <span class="money">&yen;{{food.min_price}}</span>
                  <span class="unit">/{{food.unit}}</span>
                </p>
              </div>
              <app-cartcontrol :food="food"></app-cartcontrol>
            </li>
          </ul>
        </li>
        <!-- 其他分类的商品 end -->
      </ul>
    </div>
    <!-- 商品列表 end -->

    <!-- 购物车 start -->
    <app-shopcart :poiInfo="poiInfo" :selectedFood="selectedFood"></app-shopcart>
    <!-- 购物车 end -->

    <!-- 商品详情页 start -->
    <app-product-detail :food="clickedFood" ref="productDetail"></app-product-detail>
    <!-- 商品详情页 end -->
  </div>
</template>

<script>
  import BScroll from 'better-scroll'
  import Shopcart from '../shopcart/Shopcart'
  import Cartcontrol from '../cartcontrol/Cartcontrol'
  import ProductDetail from '../productDetail/ProductDetail'

  export default {
    components: {
      'app-shopcart': Shopcart,
      'app-cartcontrol': Cartcontrol,
      'app-product-detail': ProductDetail
    },
    data(){
      return {
        container: {},
        goods: [],
        poiInfo: {},
        menuScroll: {},
        foodScroll: {},
        heightList: [],
        scrollY: 0,
        clickedFood: {}
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

            // DOM 已更新
            this.$nextTick(() => {
              this.initScroll()

              // 计算分类的区间高度 -> 监听滚动的位置 -> 根据滚动位置确定下标，实现左侧样式 -> 通过下标实现：点击左侧，滚动右侧
              this.calHeight()
            })
          }
        })
    },
    computed: {
      currentIndex(){
        for(let i = 0, len = this.heightList.length; i < len; i ++){
          let heightStart = this.heightList[i]
          let heightEnd = this.heightList[i + 1]

          if(!heightEnd || this.scrollY >= heightStart && this.scrollY < heightEnd){
            return i;
          }
        }
      },
      selectedFood(){
        let foods = []
        this.goods.forEach((classifyFood) => {
          classifyFood.spus.forEach(food => {
            if(food.count > 0){
              foods.push(food)
            }
          })
        })
        return foods
      }
    },
    methods: {
      initScroll(){
        this.menuScroll = new BScroll(this.$refs.menuScroll, {
          click: true
        })
        this.foodScroll = new BScroll(this.$refs.foodScroll, {
          probeType:3,
          click: true
        })

        this.foodScroll.on('scroll', (pos) =>{
          this.scrollY = Math.abs(Math.round(pos.y));
        })
      },
      calHeight(){
        let foodList = this.$refs.foodScroll.getElementsByClassName('food-list-hook')

        let height = 0
        this.heightList.push(height)

        for(let i = 0, len = foodList.length; i < len; i ++){
          height += foodList[i].clientHeight
          this.heightList.push(height)
        }
      },
      selectMenu(index){
        let foodList = this.$refs.foodScroll.getElementsByClassName('food-list-hook')
        this.foodScroll.scrollToElement(foodList[index], 250)
      },
      calCount(spus){
        let count = 0;
        spus.forEach((food) => {
          if(food.count > 0){
            count += food.count
          }
        })
        return count
      },
      showDetail(food){
        this.clickedFood = food
        this.$refs.productDetail.showProduct()   // 父组件直接执行子组件的方法
      }
    }
  }
</script>

<style scoped>
  .goods{
    display: flex;
    width: 100%;
    position: absolute;
    top: 3.4rem;
    bottom: 1rem;
    overflow: hidden;
  }
  .goods .menu-wrapper{
    flex: 0 0 1.5rem;
    background: #f4f4f4;
  }
  .goods .menu-wrapper .menu-item{
    position: relative;
    padding: 0.3rem 0.3rem 0.3rem 0.2rem;
    border-bottom: 1px solid #e4e4e4;
  }
  .goods .menu-wrapper .menu-item.current{
    background: #fff;
  }
  .goods .menu-wrapper .menu-item .text{
    line-height: 0.35rem;
    font-size: 13px;
    -webkit-line-clamp: 2;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    text-overflow: ellipsis;
    overflow: hidden;
  }
  .goods .menu-wrapper .menu-item .icon{
    width: 0.3rem;
    height: 0.3rem;
    vertical-align: sub;
  }
  .goods .menu-wrapper .menu-item i{
    position: absolute;
    right: 0.1rem;
    top: 0.1rem;
    width: 0.3rem;
    height: 0.3rem;
    line-height: 0.3rem;
    border-radius: 50%;
    text-align: center;
    background: #e90515;
    color: #fff;
    font-style: normal;
  }
  .goods .foods-wrapper{
    flex: 1;
    width: 6rem;
  }
  .goods .foods-wrapper .container-list{
    padding: 0.2rem 0.2rem 0 0.2rem;
    border-bottom: 1px solid #e4e4e4;
  }
  .goods .foods-wrapper .container-list img{
    width: 100%;
    margin-bottom: 0.2rem;
  }
  .goods .food-list{
    padding: 0.2rem;
  }
  .goods .food-list .classify-name{
    position: relative;
    line-height: 0.26rem;
    margin-bottom: 0.24rem;
    padding-left: 0.14rem;
    font-size: 13px;
  }
  .goods .food-list .classify-name:before{
    content: '';
    position: absolute;
    top: 0.02rem;
    left: 0;
    width: 0.05rem;
    height: 0.2rem;
    background: #ec8b06;
    border-radius: 3px;
  }
  .goods .food-list .food-item{
    position: relative;
    display: flex;
    margin-bottom: 0.4rem;
  }

  .goods .food-list .food-item img{
    flex: 0 0 1.3rem;
    width: 1.3rem;
    height: 1.3rem;
    margin-right: 0.2rem;
  }
  .goods .food-list .food-item .content{
    flex: 1;
  }
  .goods .food-list .food-item .content p, .goods .food-list .food-item .content span{
    color: #bfbfbf;
  }
  .goods .food-list .food-item .content p{
    line-height: 0.28rem;
    margin: 0.08rem 0;
  }
  .goods .food-list .food-item .name{
    font-size: 16px;
  }
  .goods .food-list .food-item .desc{
    -webkit-line-clamp: 1;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    text-overflow: ellipsis;
    overflow: hidden;
  }
  .goods .food-list .food-item .detail .saled{
    margin-right: 0.2rem;
  }
  .goods .food-list .food-item .recommend-icon{
    height: 0.3rem;
  }
  .goods .food-list .food-item .price .money{
    font-weight: bold;
    color: #fb4e44;
  }
</style>
