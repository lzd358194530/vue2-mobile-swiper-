<template>
  <div>
        <swipe v-model="index" class="swiper-wrap" :time="time">
          <swipe-item>
            <img src="1.jpg" class="banner">
          </swipe-item>
          <swipe-item>
            <img src="2.jpg" class="banner">
          </swipe-item>
          <swipe-item>
            <img src="3.jpg" class="banner">
          </swipe-item>
        </swipe>
  </div>
</template>

<script>
import Swipe from '../../components/swipe/swipe.vue';
import SwipeItem from '../../components/swipe/swipe-item.vue';
  export default {
    data() {
      return {
        index: 0,//当前的显示第几张
        time:3000//单位 毫秒 为0 则不自动轮播
      }
    }
  }
</script>

<style lang="less" scoped>
  .slider{
    position:relative;
    height:8.74666667rem;
    overflow:hidden;
  }
  .swiper-wrap,.swiper-wrap .banner{
    width:100%;
    height:100%
  }
  
</style>
