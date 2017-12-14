<template>
  <div>
    <bar>
      <span slot="title">公告栏</span>
    </bar>
    <div class="content bg-gray" ref="scrollBox">
      <div class="slider" @click="bindBannerClick">
        <swipe v-model="index" class="swiper-wrap" :time="time">
          <swipe-item>
            <img src="../../util/images/Board-banner1.jpg" class="banner">
          </swipe-item>
          <swipe-item>
            <img src="../../util/images/Board-banner2.jpg" class="banner">
          </swipe-item>
          <swipe-item>
            <img src="../../util/images/Board-banner3.jpg" class="banner">
          </swipe-item>
        </swipe>
        <!-- <mu-flat-button label="查看介绍" class="flat-button border-yellow opacity" labelPosition="before"
        labelClass="yellow" to="./buy">
          <i class="icon-arrow2"></i>
        </mu-flat-button> -->
      </div>
      <div class="bulletin mb5">
        <div class="bulletin-title bg-orange white">
          <img src="../../util/images/gold.png" class="img-bulletin"><span>奖金池</span>
        </div>
        <div class="bulletin-info money" v-if="status==0">
          <p class="orange">￥ {{account}}</p>
          <p class="orange bulletin-time">{{sendtime}} 开始发放</p>
        </div>
        <div class="bulletin-info bulletin-text" v-if="status==1">
            <img src="../../util/images/icn-Issued.png" class="img-clock"><span class="orange">奖金池已发放，请注意查收！</span>
        </div>
        <div class="bulletin-info bulletin-text" v-if="status==2">
            <img src="../../util/images/icn-forward.png" class="img-clock"><span class="orange">奖金池即将开放，敬请期待！</span>
        </div>
     
      </div>
      <ul v-for="info in infoList">
        <li class="mb5" :id="info.id">
          <card :info="info"></card>
        </li>
      </ul>
      <!-- <mu-infinite-scroll :scroller="scroller" :loading="loading" @load="loadMore"/> -->
    </div>
  </div>
</template>

<script>
import Swipe from '../../components/swipe/swipe.vue';
import SwipeItem from '../../components/swipe/swipe-item.vue';
  export default {
    data() {
      return {
        index: 0,
        loading: false,
        scroller: null,
        isSwipe:true,
        status:-1, //是否发放奖金
        account:'',
        sendtime:'',
        infoList:[{
            icon:'',
            title:'',
            text:'',
            time:''
          }]
      }
    },
    activated(){
      this.getData()
      this.isSwipe = true;
    },
    deactivated(){
      this.isSwipe = false;
    },
    methods:{
      bindBannerClick(){
        let idx = this.index;
        switch(idx){
          case 0:
            this.$router.push({
              path:'./introduction1'
            })
            break;
          case 1:
            this.$router.push({
              path:'./introduction2'
            })
            break;
          case 2:
            this.$router.push({
              path:'./introduction3'
            })
            break;
          default:
            return false
        }
      },
      // loadMore () {
      //   this.loading = true
      //   setTimeout(() => {
      //     this.loading = false
      //   }, 2000)
      // },
      getData(){
        this.Indicator.open({
          spinnerType: 'snake'
        });
        this.get({
          url:'notice',
          cb:(res)=>{
            // this.Indicator.close();
            let {code,msg,data} = res;
            if(code == 200){
              this.infoList = data.map(item=>{
                let icon = '',title = '';
                if(item.type ==1){
                  icon = 'icon-bonus';
                  title = '每日奖金'
                }
                if(item.type ==2){
                  icon = 'icon-system';
                  title = '系统公告'
                }
                return {
                  icon,
                  title,
                  text:item.msg,
                  id:item.id,
                  time:item.ctime
                }
              })
              this.getTotal()
            }else{
              this.Toast({
                message: msg,
                duration: 1000
              });
            }  
          }
        })
      },
      getTotal(){//奖金池
        this.get({
          url:'Home/notices',
          cb:(res)=>{
            this.Indicator.close();
            let {code,msg,data} = res;
            if(code == 200){
              let {account,status,time} = data;
              this.account = account;
              this.sendtime = time;
              this.status = status;
            }else{
              
            }  
          }
        })
      },
    },
    mounted () {
      //this.scroller = this.$refs.scrollBox
    },
    computed:{
      time(){
        return this.isSwipe?5000:0
      }
    },
    components:{
      Card:()=> import ('../../components/card'),
      Swipe,
      SwipeItem
    }
  }
</script>

<style lang="less" scoped>
  .content{
  }
  .slider{
    position:relative;
    height:8.74666667rem;
    overflow:hidden;
  }
  .swiper-wrap,.swiper-wrap .banner{
    width:100%;
    height:100%
  }
  .flat-button{
    position:absolute;
    z-index: 99;
    right:.85333333rem;
    bottom:.85333333rem;
  }
  .bulletin{
    background-color: #fff;
    padding: .64rem 1.49333333rem;
    text-align: center;
    .bulletin-title{
      height:1.664rem;
      display: flex;
      justify-content:center;
      align-items: center;
    }
    .img-bulletin{
      width:1.19466667rem;
      height:1.19466667rem;
      margin-right: 5px;
    }
    .img-clock{
      width:1.87733333rem;
      height:1.87733333rem;
      margin-right: 10px;
    }
    .bulletin-info{
      height:4.05333333rem;
      border:1px solid #f8a219;
    }
    .bulletin-text{
      display:flex;
      justify-content:center;
      align-items: center;
    }
    .money{
      padding-top: .8rem;
    }
    .bulletin-time{
      line-height: 1.28rem;
      height:1.28rem;
      display:inline-block;
      padding:0 10px;
      border:1px solid #f8a219;
      border-radius:5px;
      margin-top:.32rem;
    }
  }
</style>