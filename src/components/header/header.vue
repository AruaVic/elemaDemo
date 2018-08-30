<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar"><img :src="seller.avatar" alt="" width="64" height="64"/></div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">{{seller.description}}/{{seller.deliveryTime}}分钟送达</div>
        <div v-if="seller.supports" class="support">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div v-if="seller.supports" class="support-count" @click="showDetail">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon-circle-right"></i>
      </div>
    </div>
    <div class="bulletin-wrapper" @click="showDetail">
      <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon-circle-right"></i>
    </div>
    <div class="background"><img :src="seller.avatar" width="100%" height="100%" alt=""/></div>
    <transition name="fade">
      <div v-show="detailShow" class="detail">
      <div class="detail-wrapper clearfix">
        <div class="detail-main">
          <h1 class="name">{{seller.name}}}</h1>
          <div class="star-warpper">
            <star :score="seller.score" :size="48"/>
          </div>
          <div class="title">
            <div class="line"></div>
            <div class="text">优惠信息</div>
            <div class="line"></div>
          </div>
          <ul v-if="seller.supports" class="supports">
            <li class="support-item" v-for="(item,index) in seller.supports">
              {{item.type}}
              <span class="icon" :class="classMap[item.type]"></span>
              <span class="text">{{item.description}}</span>
            </li>
          </ul>
          <div class="title">
            <div class="line"></div>
            <div class="text">商家公告</div>
            <div class="line"></div>
          </div>
          <div class="bulletin">
            <p class="content">{{seller.bulletin}}</p>
          </div>
        </div>
      </div>

      <div class="detail-close" @click="showDetail"><i class="icon-blocked"></i></div>
    </div>
    </transition>
  </div>
</template>
<script>
  import star from '../star/star'
  export default{
    name:'header',
    data(){
      return{
        classMap:[],
        detailShow:false
      }
    },
    props:{
      seller:{
        type:Object,
        default:{}
      }
    },
    created(){
      this.classMap=['decrease','discount','special','invoice','gurantee']
    },
    methods:{
      showDetail(){
        return this.detailShow=!this.detailShow
      }
    },
    components:{
      star
    }
  }
</script>
<style scoped>
  .header{
    position: relative;
    overflow: hidden;
    color:#fff;
    background-color: rgba(7,17,25,0.5);
  }
  .content-wrapper{
    position: relative;
    padding:24px 12px 18px 24px;
    font-size: 0;
  }
  .content-wrapper .avatar{
    display: inline-block;
    vertical-align: top;
  }
  .content-wrapper .avatar img{
    border-radius: 2px;
  }
  .content-wrapper .content{
    display: inline-block;
    font-size: 14px;
    margin-left:16px;
  }
  .title{
    margin: 2px 0 8px 0;
  }
  .title .brand{
    display: inline-block;
    width:30px;
    height:18px;
    background-image: url('./brand@2x.png');
    background-size: 30px 18px;
    vertical-align: middle;
  }
  .title .name{
    margin-left: 6px;
    font-size: 16px;
    font-weight: bold;
    line-height: 16px;
  }
  .description{
    margin-bottom: 10px;
    font-size: 12px;
    line-height: 12px;
  }
  .support .icon{
    display: inline-block;
    width:12px;height:12px;
    background-image: url(./decrease_1@2x.png);
    background-repeat: no-repeat;
    background-size: 12px 12px;
    margin-right:4px;
    vertical-align: middle;
  }
  .support .icon.decrease{
    background-image: url(./decrease_1@2x.png);
  }
  .support .icon.discount{
    background-image: url(./discount_1@2x.png);
  }
  .support .icon.gurantee{
    background-image: url(./guarantee_1@2x.png);
  }
  .support .icon.invoice{
    background-image: url(./invoice_1@2x.png);
  }
  .support .icon.special{
    background-image: url(./special_1@2x.png);
  }
  .support .text{
    font-size: 12px;
    line-height: 10px;
  }
  .support-count{
    position: absolute;
    right:12px;
    bottom:8px;
    padding:0 8px;
    height:24px;
    line-height: 24px;
    border-radius: 16px;
    background-color:rgba(0,0,0,0.2);
  }
  .support-count span.count,.support-count i{
    font-size: 10px;
  }
  .support-count i{
    line-height: 24px;
    margin-left: 2px;
  }

  .bulletin-wrapper{
    position: relative;
    height:28px;
    line-height: 28px;
    padding:0 22px 0 12px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow:ellipsis;
    background-color: rgba(7,17,27,0.2);
  }
  .bulletin-title{
    display: inline-block;
    width:22px;height:12px;
    margin-right: 4px;
    vertical-align: top;
    margin-top: 8px;
    background-image: url('./bulletin@2x.png');
    background-size:22px 12px;
  }
  .bulletin-text{
    font-size: 10px;
    vertical-align: top;
  }
  .bulletin-wrapper i{
    position: absolute;
    right:12px;
    top:8px;
    font-size: 10px;
  }
  .background{
    position: absolute;
    top:0;bottom:0;
    left:0; right:0;
    z-index: -1;
    filter:blur(10px)
  }
  .detail{
    position: fixed;
    top:0;left:0;bottom: 0;right:0;
    z-index:300;
    width:100%;height:100%;
    overflow: auto;
    background: rgba(7,17,27,0.8);
  }
  .detail-wrapper{
    min-height: 100%;
    width:100%;
  }
  .detail-wrapper .detail-main{
    margin-top: 64px;
    padding-bottom: 64px;
  }
  .detail-main .name{
    line-height: 16px;
    text-align: center;
    font-size: 16px;
    font-weigth:700;
  }
  .detail-main .star-warpper{
    maring-top:18px;
    padding:2px 0;
    text-align: center;
  }
  .detail-close{
    position: relative;
    width:32px;height:32px;
    margin:-64px auto 0 auto;
    clear:both;
    font-size:32px;
  }
  .fade-enter-active,.fade-leave-active{
    transition:opacity .5s;
  }
  .fade-enter,.fade-leave-to{
    opacity: 0;
  }
  .detail-main .title{
    display:flex;
    width:80%;
    margin:28px auto 24px auto;
  }
  .detail-main .title .line{
    flex: 1;
    position: relative;
    top:-6px;
    border-bottom: 1px solid rgba(255,255,255,0.2);
  }
  .detail-main .title .text{
    padding: 0 12px;
    font-size:14px;
    font-weight: 700;
  }
  .detail-main .supports{
    width:80%;
    margin:0 auto;
  }
  .detail-main .supports .support-item{
    padding:0 12px;
    margin-bottom:12px;
    font-size:0;
  }
  .detail-main .supports .support-item:last-child{
    margin-bottom: 0;
  }
  .detail-main .supports .support-item .icon{
    display: inline-block;
    width:16px;
    height:16px;
    vertical-align: top;
    margin-right: 6px;
    background-size: 16px 16px;
    background-repeat: no-repeat;
  }
  .detail-main .supports .support-item .icon.decrease{
    background-image: url(./decrease_2@2x.png);
  }
  .detail-main .supports .support-item .icon.discount{
    background-image: url(./discount_2@2x.png);
  }
  .detail-main .supports .support-item .icon.gurantee{
    background-image: url(./guarantee_2@2x.png);
  }
  .detail-main .supports .support-item .icon.invoice{
    background-image: url(./invoice_2@2x.png);
  }
  .detail-main .supports .support-item .icon.special{
    background-image: url(./special_2@2x.png);
  }
  .detail-main .supports .support-item .text{
    line-height: 16px;
    font-size: 12px;
  }
  .bulletin{
   width:80%;
    margin:0 auto;
  }
  .bulletin .content{
    padding:0 12px;
    line-height: 24px;
    font-size: 12px;
  }


  @media (-webkit-device-pixel-ratio: 3),(min-device-pixel-ratio: 3) {
    .title .brand{
      background-image: url('./brand@3x.png');
    }
    .support .icon.decrease{
      background-image: url(./decrease_1@3x.png);
    }
    .bulletin-title{
      background-image: url('./bulletin@3x.png');
    }
  }
</style>
