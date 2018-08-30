<template>
    <div class="shopcart">
        <div class="content" @click="toggleList">
          <div class="content-left">
            <div class="logo-wrapper">
              <div class="logo" :class="{'highlight':totalCount>0}">
                <i class="icon-cart" :class="{'highlight':totalCount>0}"></i>
              </div>
              <div class="num" v-show="totalCount>0">{{totalCount}}</div>
            </div>
            <div class="price" :class="{'highlight':totalCount>0}">￥{{totalPrice}}</div>
            <div class="desc">另需配送费{{deliveryPrice}}</div>
          </div>
          <div class="content-right" @click.stop.prevent="pay"><div class="pay" :class="payClass">{{payDesc}}</div></div>
        </div>
        <div class="ball-container">
          <transition-group name="drop">
            <div :key="index" v-for="(ball,index) in balls" v-show="ball.show" class="ball">
              <div class="inner inner-hook"></div>
            </div>
          </transition-group>

        </div>
        <transition name="fold">
          <div class="shopcart-list" v-show="listShow">
            <div class="list-header">
              <h1 class="title">购物车</h1>
              <span class="empty" @click="empty">清空</span>
            </div>
            <div class="list-content" ref="listContent">
              <ul>
                <li class="food" v-for="food in selectFoods">
                  <span class="name">{{food.name}}</span>
                  <div class="price">
                    <span class="">￥{{food.price*food.count}}</span>
                  </div>
                  <div class="cartcontrol-wrapper">
                    <cartControl :food="food"/>
                  </div>
                </li>
              </ul>
            </div>
          </div>
        </transition>
        <div class="list-mask" v-show="listShow" @click="hideList"></div>
    </div>

</template>
<script>
    import cartControl from '../cartcontrol/cartcontrol'
    import BScroll from 'better-scroll'
    export default{
        name: '',
        data () {
            return {
              balls:[{
                show:false
              },{
                show:false
              },{
                show:false
              },{
                show:false
              },{
                show:false
              }],
              dropBall:[],
              fold:true
            }
        },
        props:{
          selectFoods:{
            type:Array,
            default(){
              return [{
                price:9.6,
                count:2
              }];
            }
          },
          deliveryPrice:{
            type:Number,
            default :0
          },
          minPrice:{
            type:Number,
            default :0
          }
        },
        computed:{
          totalPrice(){
            let total=0;
            this.selectFoods.forEach((food)=>{
              total += food.price * food.count
            });
            return total;
          },
          totalCount(){
            let totalCount=0;
            this.selectFoods.forEach((food)=>{
              totalCount += food.count;
            })
            return totalCount;
          },
          payDesc(){
            if(this.totalPrice===0){
              return '￥'+ this.minPrice +'元起送';
            }else if(this.totalPrice>0 && this.totalPrice<this.minPrice){
              let diff = Math.floor(this.minPrice-this.totalPrice);
              return '还差'+diff+'元起送';
            }
            return '结算';
          },
          payClass(){
            if(this.totalPrice<this.minPrice){
              return 'not-enough'
            }else{
              return 'enough'
            }
          },
          listShow(){
            if(!this.totalCount){
              this.fold=true;
              return false;
            }
            let show = !this.fold;
            if(show){
              this.$nextTick(()=>{
                if(!this.scroll){
                  this.scroll=new BScroll(this.$refs.listContent,{
                    click:true
                  })
                }else{
                  this.scroll.refresh();
                }

              })
            }
            return show;
          }
        },
        methods:{
          /*drop(el){
            console.log("********",el);
            for(let i=0;i<this.balls.length;i++){
              let ball =this.balls[i];
              if(ball.show){
                ball.show =true;
                ball.el=el;
                this.dropBall.push(ball);
                return;
              }
            }
          },
          beforeEnter(el){
            let count= this.balls.length;
            while(count--){
              let ball=this.balls[count];
              if(ball.show){
                let rect =ball.el.getBoundingClientRect();
                let x=rect.left-32;
                let y=-(window.innerHeight-rect.top-32);
                el.style.display='';
                el.style.webkitTransform=`translate3d(0,${y}px,0)`;
                el.style.transform=`translate3d(0,${y}px,0)`;
                let inner=el.getElementsByClassName('inner-hook')[0];
                inner.style.webkitTransform=`translate3d(${x}px,0,0)`;
                inner.style.transform=`translate3d(${y}px,0,0)`;
              }
            }
          },
          enter(el){
            let rf=el.offsetHeight;
            this.$nextTick(()=>{
              el.style.webkitTransform='translate3d(0,0,0)';
              el.style.transform='translate3d(0,0,0)';
              let inner=el.getElementsByClassName('inner-hook')[0];
              inner.style.webkitTransform='translate3d(0,0,0)';
              inner.style.transform='translate3d(0,0,0)';
            })
          },
          afterEnter(el){
            let ball=this.dropBalls.shift();
            if(ball){
              ball.show=false;
              el.style.display='none';
            }
          },*/
          toggleList(){
            if(!this.totalCount){
              return;
            }
            this.fold=!this.fold;
          },
          empty(){
            this.selectFoods.forEach((food)=>{
              food.count = 0;
            })
          },
          hideList(){
            this.fold=true;
          },
          pay(){
            if(this.totalPrice<this.minPrice){
              return;
            }
            alert('请支付'+this.totalPrice+'元！')
          }
        },
        components:{
          cartControl
        }
    }
</script>
<style scoped>
  .shopcart{
    position:fixed;
    left:0;
    bottom: 0;
    z-index:100;
    width:100%;
    height:48px;
    background: rgb(5,5,5);
  }
  .content{
    display:flex;
    background: #141d27;
    font-size: 0;
  }
  .content-left{
    flex: 1;
  }
  .content-left .logo-wrapper{
    box-sizing: border-box;
    display: inline-block;
    position: relative;
    top:-10px;
    width:56px;height:56px;
    margin:0 12px;
    padding:6px;
    border-radius: 50%;
    vertical-align: top;
    background: #141d27;
  }
  .logo-wrapper .logo{
    width: 100%;height:100%;
    border-radius: 50%;
    background: rgba(255,255,255,0.2);
    text-align: center;
  }
  .logo-wrapper .logo .icon-cart{
    font-size: 20px;
    color:#80858a;
    line-height: 44px;
  }
  .logo.highlight{
    background: rgb(0,160,220);
  }
  .logo .icon-cart.highlight{
    color:#fff;
  }
  .logo-wrapper .num{
    position: absolute;
    top:0;right:0;
    width:24px;
    height: 16px;
    line-height: 16px;
    text-align: center;
    border-radius: 16px;
    font-size: 9px;
    font-weight: 700;
    color: #fff;
    background: red;
    box-shadow: 0 4px 8px 0 rgba(0,0,0,0.4);
  }
  .content-left .price{
    display: inline-block;
    vertical-align: top;
    line-height: 24px;
    margin-top: 12px;
    box-sizing: border-box;
    padding-right:12px;
    border-right:1px solid rgba(255,255,255,0.1);
    font-size: 16px;
    font-weight: 700;
    color:rgba(255,255,255,0.4)
  }
  .content-left .price.highlight{
    color:#fff;
  }
  .content-left .desc{
    display: inline-block;
    vertical-align: top;
    line-height: 24px;
    margin: 12px 0 0 12px;
    font-size:10px;
    color:rgba(255,255,255,0.4)
  }
  .content-right{
    flex:0 0 105px;
    width:105px;
    background: rgba(255,255,255,0.2);
  }
  .content-right .pay{
    height:48px;
    line-height: 48px;
    text-align: center;
    font-size: 16px;
    font-weight: 700;
    color:rgba(255,255,255,0.4);
  }
  .content-right .pay.not-enough{
    background: rgba(255,255,255,0.2);
  }
  .content-right .pay.enough{
    background: #00b43c;
    color:#fff;
  }
  .ball-container{

  }
  .ball-container .ball{
    position: fixed;
    left:32px;bottom:22px;
    z-index:200;

  }
  .ball-container .ball.drop-transition{
    transition: all 0.4s linear;
  }
  .ball-container .ball.drop-transition .inner{
    width:16px;
    height:16px;
    border-radius: 50%;
    background: rgb(0,160,220);
    transition: all 0.4s linear;
  }

  .shopcart-list{
    position: absolute;
    top:0;left:0;
    z-index:-1;
    width:100%;
    background: rgb(0,0,0);
    transform:translate(0,-100%);
    transition: all 0.5s;
  }
  .shopcart-list.fold-transition{
    transition: all 0.5s;
    /*transform: translate3d(0,-100%,0);*/
    transform:translate(0,-100%);
  }
  .shopcart-list.fold-enter,.shopcart-list.fold-leave-to{
    /*transform:translate3d(0,0,0);*/
    transform:translate(0,0);
  }
  .list-header{
    height:40px;
    line-height: 40px;
    font-size: 24px;
    font-weight: 200;
    padding:0 18px;
    background: #f3f5f7;
    border-bottom:1px solid rgba(7,17,27,0.1);
  }
  .list-header .title{
    float:left;
    font-size: 14px;
    color:rgb(1,17,27);
    margin: 0 auto;
  }
  .list-header .empty{
    float:right;
    font-size: 12px;
    color:rgb(0,160,220)
  }
  .list-content{
    padding:0 18px;
    max-height:217px;
    background: #fff;
    overflow: hidden;
  }
  .list-content .food{
    position:relative;
    padding:12px 0;
    box-sizing: border-box;
    border-bottom:1px solid rgba(17,27,37,0.2);
  }
  .list-content .food .name{
    line-height: 24px;
    font-size:  14px;
    color:rgb(7,17,27)
  }
  .list-content .food .price{
    position: absolute;
    right:90px;
    bottom:12px;
    font-size: 14px;
    line-height: 24px;
    font-weight: 700;
    color:red;
  }
  .list-content .food .cartcontrol-wrapper{
    position: absolute;
    right:0;
    bottom:6px;
  }
  .list-mask{
    position: fixed;
    top:0;
    left:0;
    width:100%;height:100%;
    z-index:-2;
    background: rgba(7,17,27,0.6);
  }

</style>
