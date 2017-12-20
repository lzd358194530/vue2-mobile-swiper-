<template>
      <div class="slider" @click="bindBannerClick">
        <swipe v-model="index" class="swiper-wrap" :time="time">
          <swipe-item>
            <img src="../../util/images/index-banner1.jpg" class="banner">
          </swipe-item>
          <swipe-item>
            <img src="../../util/images/index-banner2.jpg" class="banner">
          </swipe-item>
          <swipe-item>
            <img src="../../util/images/index-banner3.jpg" class="banner">
          </swipe-item>
          <swipe-item>
            <img src="../../util/images/index-banner4.jpg" class="banner">
          </swipe-item>
          <swipe-item>
            <img src="../../util/images/index-banner5.jpg" class="banner">
          </swipe-item>
        </swipe>
      </div>
</template>

<script>
  import Swipe from '../../components/swipe/swipe.vue';
  import SwipeItem from '../../components/swipe/swipe-item.vue';
  export default{
    data(){
      return {
        isSwipe:true,
        index: 0
      }
    },
    methods:{
      bindBannerClick(){
        let idx = this.index;
        switch(idx){
          //点击事件
          case 0:
          case 1:
          case 2:
          case 3:
          case 4:
          default:
            return false
        }
      }
    },
    components:{
      Swipe,
      SwipeItem
    },
   
    computed:{
      time(){
        return this.isSwipe?5000:0
      },
    },
    activated(){
      this.isSwipe = true;
    },
    deactivated(){
      this.isSwipe = false;
    }
  }
</script>

<style lang="less" scoped>
/* 轮播 */
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
