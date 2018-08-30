<template>
    <div class="sellerContent" ref="sellerContent">
      <div class="">
        <div class="title">
          <div class="title-top">
            <div class="title-left">
              <span class="shopTitle">{{seller.name}}</span>
              <div class="shopLevel">
                <Star class="star" :score="seller.score" :size="36"/>
                <span>({{seller.ratingCount}})</span>
                <span>月售{{seller.sellCount}}单</span>
              </div>
            </div>
            <div class="title-right" @click="collectToggle">
              <i class="icon-heart" :class="collect?'red':''"></i>
              <span class="collect">{{collect ? '已收藏':'收藏'}}</span>
            </div>
          </div>
          <ul class="title-bottom">
            <li>
              <div class="content">起送价</div>
              <div class="price">{{seller.minPrice}}<i class="yuan">元</i></div>
            </li>
            <li>
              <div class="content">商家配送</div>
              <div class="price">{{seller.deliveryPrice}}<i class="yuan">元</i></div>
            </li>
            <li>
              <div class="content">平均配送时间</div>
              <div class="price">{{seller.deliveryTime}}<i class="yuan">分钟</i></div>
            </li>
          </ul>
        </div>
        <div class="active">
          <div class="caption">公告与活动</div>
          <div class="notice">{{seller.bulletin}}</div>
          <ul>
            <li class="activeItem" v-for="item in seller.supports">
              <span class="icon" :class="classMap[item.type]"></span>
              <p>{{item.description}}</p>
            </li>
          </ul>
        </div>
        <div class="photo">
          <div ref="photo">
            <div class="caption">商家实景</div>
            <div>
              <div class="photoItemBox">
                <div class="photoItem" v-for="item in seller.pics"><img :src="item" alt="" width="120" height="90"/></div>
              </div>
            </div>
          </div>

        </div>
        <div class="shopsInfo">
          <div class="caption">商家信息</div>
          <ul>
            <li v-for="item in seller.infos">{{item}}</li>
          </ul>
        </div>
      </div>
    </div>
</template>
<script>
    import Star from '../star/star';
    import BScroll from 'better-scroll'
    export default{
      name: 'seller',
      data () {
        return {
          seller:{},
          collect:false,
          classMap:['decrease','discount','special','invoice','gurantee']
        }
      },
      created(){
        this.axios.get('../static/data.json').then((response)=>{
          if(response.statusText==='OK'){
            this.seller=response.data.seller;
          }else{
            alert('数据请求失败！');
          }
        }),
        this.$nextTick(()=>{
          this.InitPhotoScroll()
        })
      },
      components:{
        Star
      },
      methods:{
        collectToggle(){
          this.collect=!this.collect;
        },
        InitPhotoScroll(){
          this.$nextTick(()=>{
            if(!this.scroll){
              this.scroll=new BScroll(this.$refs.photo,{
                scrollX: true,
                scrollY: false,
                momentum: false,
                snap: {
                  loop: this.loop,
                  threshold: 0.3,
                  speed: 400
                },
                click: true
              })
              this.scroll2 =new BScroll(this.$refs.sellerContent,{
                click:true
              })
            }else{
              this.scroll.refresh();
              this.scroll2.refresh();
            }
          })

        }
      }
    }
</script>
<style scoped>
  .sellerContent{
    /*display: flex;*/
    position:absolute;
    top:174px;bottom:10px;
    width:100%;
    overflow: hidden;
  }
  .title{
    padding:18px;
    border-bottom: 1px solid rgba(147,153,159,0.2);
  }
  .title-top{
    padding-bottom: 18px;;
    border-bottom:1px solid rgba(147,153,159,0.2);
    overflow: hidden;
  }
  .title-left{
    float: left;
  }
  .title-left .shopTitle{
    font-size: 16px;
    color:rgb(7,17,27);
    line-height: 16px;
  }
  .title-left .shopLevel{
    margin-top: 8px;
  }
  .title-left .shopLevel .star{
    display: inline-block;
    margin-right: 8px;
  }
  .title-left .shopLevel span:first-child{
    margin-right: 12px;
  }
  .title-left .shopLevel span{
    font-size: 10px;
    color:rgb(77,85,93);
    line-height: 10px;
  }
  .title-right{
    float: right;
    display: flex;
    flex-direction:column;
    text-align: center;
  }
  .title-right .icon-heart{
    margin-bottom: 8px;
  }
  .title-right .icon-heart.red{
    color:rgb(240,20,20)
  }
  .title-right .collect{
    font-size:10px;
    line-height: 10px;
    color:rgb(77,85,93);
  }

  .title-bottom{
    display: flex;
    padding-top:18px;
  }
  .title-bottom li{
    width: 33%;
  }

  .title-bottom li:nth-child(2){
    border-right:1px solid rgb(147,153,159);
    border-left:1px solid rgb(147,153,159);
  }
  .title-bottom li .content{
    text-align: center;
    font-size: 10px;
    color:rgb(147,153,159);
    line-height: 10px;
    margin-bottom: 8px;
  }
  .title-bottom li .price{
    text-align: center;
    font-size: 24px;
    font-weight: 100;
    color:rgb(7,17,27);
    line-height: 24px;
  }
  .title-bottom li .price i{
    font-style:normal;
    font-size: 10px;

  }
  .active{
    margin: 16px auto;
    padding:16px;
    border-bottom: 1px solid rgba(7,17,27,0.1);
  }
  .caption{
    font-size: 14px;
    color:rgb(7,17,27);
    line-height: 14px;
  }
  .active .notice{
    padding:16px 12px 16px 12px;
    font-size: 12px;
    font-weight: 100;
    color:rgb(240,20,20);
    line-height: 24px;
  }

  .active .activeItem{
    padding:16px 12px;
    border-bottom: 1px solid rgba(7,17,27,0.1);
  }
  .active .activeItem:last-child{
    border-bottom: none;
  }

  .activeItem .icon{
    display: inline-block;
    width:16px;height:16px;
    background-repeat: no-repeat;
    background-size: 16px 16px;
    margin-right:6px;
    vertical-align: middle;
  }
  .activeItem .icon.decrease{
    background-image: url(./decrease_4@2x.png);
  }
  .activeItem .icon.discount{
    background-image: url(./discount_4@2x.png);
  }
  .activeItem .icon.gurantee{
    background-image: url(./guarantee_4@2x.png);
  }
  .activeItem .icon.invoice{
    background-image: url(./invoice_4@2x.png);
  }
  .activeItem .icon.special{
    background-image: url(./special_4@2x.png);
  }

  .activeItem p{
    display: inline-block;
    font-size: 12px;
    color:rgb(7,17,27);
    line-height: 16px;
    font-weight: 100;
  }

  .photo{
    padding:18px;
    border-top:1px solid rgba(7,17,27,0.1);
    border-bottom:1px solid rgba(7,17,27,0.1);
  }
  .photoItemBox{
    margin-top: 12px;;
    height: 90px;
    display: flex;
    overflow: hidden;
  }
  .photoItemBox .photoItem{
    margin-right: 6px;
  }

  .shopsInfo{
    border-top:1px solid rgba(7,17,27,0.1);
    border-bottom:1px solid rgba(7,17,27,0.1);
    margin-top: 16px;
    padding:18px;
  }
  .shopsInfo ul{
    margin-top: 12px;
  }
  .shopsInfo li{
    padding:16px 12px;
    border-top:1px solid rgba(7,17,27,0.1);
    font-size: 12px;
    font-weight: 100;
    line-height: 16px;
    color:rgb(7,17,27);
  }

</style>
