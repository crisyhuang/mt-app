<template>
  <span class="star">
    <span class="star-item" v-for="(itemClass, index) in itemClasses" :key="index" :class="itemClass"></span>
  </span>
</template>

<script>
  const LENGTH = 5
  const STAR_ON = 'on'
  const STAR_HALF = 'half'
  const STAR_OFF = 'off'

  export default {
    props: {
      score: {
        type: Number
      }
    },
    computed: {
      itemClasses(){
        let result = [];
        let hasHalf = this.score % 1 !== 0
        let onLen = Math.floor(this.score)

        for(let i = 0; i < onLen; i ++){
          result.push(STAR_ON)
        }

        if(hasHalf){
          result.push(STAR_HALF)
        }

        while(result.length < LENGTH){
          result.push(STAR_OFF)
        }

        return result
      }
    }
  }
</script>

<style scoped>
  .star{
    font-size: 0;
  }
  .star .star-item{
    display: inline-block;
    width: 0.2rem;
    height: 0.2rem;
    background-repeat: no-repeat;
    background-size: 0.2rem 0.2rem;
  }
  .star .on{
    background-image: url("./img/star24_on@2x.png");
  }
  .star .half{
    background-image: url("./img/star24_half@2x.png");
  }
  .star .off{
    background-image: url("./img/star24_off@2x.png");
  }
</style>
