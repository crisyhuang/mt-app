<template>
  <div id="app">
    <!-- header -->
    <app-header :poiInfo="poiInfo"></app-header>

    <!-- nav -->
    <app-nav></app-nav>

    <!-- content -->
    <keep-alive>
      <router-view></router-view>
    </keep-alive>
  </div>
</template>

<script>
import Header from './components/header/Header'
import Nav from './components/nav/Nav'

export default {
  name: 'App',
  data(){
    return {
      poiInfo: {}
    }
  },
  components: {
    'app-header': Header,
    'app-nav': Nav
  },
  created(){
    fetch("/api/goods")
      .then(res => {
      return res.json()
    })
      .then(res => {
        if(res.code === 0){
          this.poiInfo = res.data.poi_info
        }
    })
  }
}
</script>

<style>

</style>
