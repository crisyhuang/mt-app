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
            <li class="food-item" v-for="(food, index) in item.spus" :key="index">
              <img :src="food.picture">
              <div class="content">
                <h3 class="name">{{food.name}}</h3>
                <p class="desc">{{food.description}}</p>
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
            </li>
          </ul>
        </li>
        <!-- 其他分类的商品 end -->
      </ul>
    </div>
    <!-- 商品列表 end -->
  </div>
</template>

<script>
  import BScroll from 'better-scroll'

  export default {
    data(){
      return {
        container: {},
        goods: [],
        poiInfo: {},
        menuScroll: {},
        foodScroll: {},
        heightList: [],
        scrollY: 0
      }
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
    z-index: -1;
  }
  .goods .menu-wrapper{
    flex: 0 0 1.5rem;
    background: #f4f4f4;
  }
  .goods .menu-wrapper .menu-item{
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
    margin-bottom: 0.08rem;
  }
  .goods .food-list .food-item .name{
    font-size: 16px;
    margin-bottom: 0.2rem;
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
    margin-bottom: 0.1rem;
  }
  .goods .food-list .food-item .price .money{
    font-weight: bold;
    color: #fb4e44;
  }




</style>
