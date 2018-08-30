<template>
    <div class="goods">
      <div class="menu-wrapper" ref="menuWrapper">
        <ul class="menu-ul">
          <li v-for="(item,index) in goods" class="menu-item" :class="{'current':currentIndex==index}" @click="selectMenu(index,$event)">
            <span class="text border-1px"><span class="icon" v-show="item.type>0" :class="classMap[item.type]"></span>{{item.name}}</span>
          </li>
        </ul>
      </div>
      <div class="foods-wrapper" ref="foodWrapper">
        <ul>
          <li v-for="item in goods" class="food-list food-list-hook">
            <h1 class="title">{{item.name}}</h1>
            <ul>
              <li v-for="food in item.foods" class="food-item border-1px">
                <div class="icon"><img :src="food.icon" width="57" height="57" alt=""/></div>
                <div class="content">
                  <h2 class="name">{{food.name}}</h2>
                  <p class="desc">{{food.description}}</p>
                  <div class="extra">
                    <span class="count">月售{{food.sellCount}}份</span>
                    <span>好评率{{food.rating}}%</span>
                  </div>
                  <div class="price">
                    <span class="now">￥{{food.price}}</span>
                    <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                  </div>
                  <div class="cartcontrol-wrapper">
                    <cartControl :food="food" @cart.add="cardAdds"/>
                  </div>
                </div>
              </li>
            </ul>
          </li>
        </ul>
      </div>
      <shopCart ref="shopCart" :selectFoods="selectFoods" :deliveryPrice="seller.deliveryPrice" :minPrice="seller.minPrice"/>
    </div>
</template>
<script>
  import BScroll from 'better-scroll'
  import shopCart from '../shopcart/shopcart'
  import cartControl from '../cartcontrol/cartcontrol'
    export default{
        name: 'goods',
        data () {
            return {
              goods:[],
              classMap:[],
              listHeight:[],
              scrollY:0
            }
        },
        props:{
          seller:{
            type:Object
          }
        },
        created(){
          this.classMap=['decrease','discount','special','invoice','gurantee']
          this.axios.get('../static/data.json').then((response) => {
            //console.log('goods',response);
            if(response.statusText=='OK'){
              this.goods=response.data.goods;
              this.$nextTick(() => {
                this._initScroll();
                this._calculateHeight();
              })
            }else{
              console.log('数据请求失败！')
            }
          })
        },
        computed:{
          currentIndex(){
            for(let i=0;i<this.listHeight.length;i++){
              let height1=this.listHeight[i];
              let hegiht2=this.listHeight[i+1];
              if(!hegiht2 ||(this.scrollY>=height1 && this.scrollY<hegiht2)){
                return i;
              }
            }
            return 0;
          },
          selectFoods(){
            let foods=[];
            this.goods.forEach((good)=>{
              good.foods.forEach((food)=>{
                if(food.count>0){
                  foods.push(food);
                }
              })
            })
            return foods;
          }
        },
        methods:{
          _initScroll(){
            this.menuScroll =new BScroll(this.$refs.menuWrapper,{
              click:true
            });
            this.foodScroll =new BScroll(this.$refs.foodWrapper,{
              probeType:3,
              click:true
            })
            this.foodScroll.on('scroll',(pos)=>{
              this.scrollY=Math.abs(Math.round(pos.y));
            })
          },
          _calculateHeight(){
            let foodList=this.$refs.foodWrapper.getElementsByClassName('food-list-hook');
            let height=0;
            this.listHeight.push(height);
            for(let i=0;i<foodList.length;i++){
              let item=foodList[i]
              height+=item.clientHeight;
              this.listHeight.push(height);
            }
          },
          selectMenu(index,event){
            if(!event._constructed){
              return
            }
            let foodList=this.$refs.foodWrapper.getElementsByClassName('food-list-hook');
            let el=foodList[index];
            this.foodScroll.scrollToElement(el,600)
          },
          cardAdds(target){
            this.$refs.shopCart.drop(target);
          }
        },
        components:{
          shopCart,
          cartControl
        },
        events:{
          'cart.add'(target){
            this._drop(target)
          }
        }
    }
</script>
<style scoped>
  @import "../../common/styles/mixin.css";
  .goods{
    display: flex;
    position:absolute;
    top:174px;bottom:46px;
    width:100%;
    overflow: hidden;
  }
  .menu-wrapper{
    flex: 0 0 80px;
    width:80px;
    background: #f3f5f7;
  }
  .menu-item{
    display: table;
    vertical-align: middle;
    padding:0 12px;
    height:54px;
    width:56px;
    line-height: 14px;
  }
  .menu-item.current{
    position:relative;
    z-index:10;
    margin-top: -1px;
    background: #fff;
    font-weight: 700;
  }
  .menu-item.current .text{
    border:none;
  }
  .menu-item .text{
    display: table-cell;
    width:56px;
    vertical-align: middle;
    font-size: 12px;
  }
  .menu-item .icon{
    display: inline-block;
    width:12px;height:12px;
    background-repeat: no-repeat;
    background-size: 12px 12px;
    margin-right:2px;
    vertical-align: middle;
  }
  .menu-item .icon.decrease{
    background-image: url(./decrease_3@2x.png);
  }
  .menu-item .icon.discount{
    background-image: url(./discount_3@2x.png);
  }
  .menu-item .icon.gurantee{
    background-image: url(./guarantee_3@2x.png);
  }
  .menu-item .icon.invoice{
    background-image: url(./invoice_3@2x.png);
  }
  .menu-item .icon.special{
    background-image: url(./special_3@2x.png);
  }






  .foods-wrapper{
    flex:1;
  }
  .foods-wrapper .title{
    padding-left:14px;
    height:26px;
    line-height: 26px;
    border-left:2px solid #d9dde1;
    font-size: 12px;
    color:rgb(147,153,159);
    background: #f3f5f7;
  }
  .food-item{
    display: flex;
    margin: 18px;
    padding-bottom: 18px;
  }
  .food-item:last-child{
    margin-bottom: 0;
  }
  .border-1px:last-child{
    border-top:none;
  }
  .food-item .icon{
    flex: 0 0 57px;
    margin-right:10px;
  }
  .food-item .content{
    flex: 1;
  }
  .food-item .content .name{
    margin:2px 0 8px 0;
    line-height: 10px;
    font-size: 10px;
    color: rgb(7,17,27);
  }
  .content .desc{
    margin-bottom: 8px;
    line-height: 12px;
    font-size: 10px;
    color:rgb(7,17,27);
  }
  .content .extra{
    line-height: 10px;
    font-size: 10px;
    color:rgb(7,17,27);
  }
  .content .extra .count{
    margin-right: 12px;
  }
  .price{
    font-weight: 700;
    line-height: 24px;
  }
  .price .now{
    color:rgb(240,0,0);
    margin-right:8px;
    font-size: 14px;
  }
  .price .old{
    text-decoration: line-through;
    font-size: 10px;
    color:rgb(147,153,159);
  }
  .cartcontrol-wrapper{
    position: absolute;
    right:0;bottom:12px;
  }
</style>
