<template>
    <div class="cartcontrol">
      <transition name="move">
        <div class="cart-decrease icon-minus" v-show="food.count>0" @click="decreaseCart">
          <div class="inner icon-minus"></div>
        </div>
      </transition>
      <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
      <div class="cart-add icon-plus" @click="addCart"></div>
    </div>
</template>
<script>
  import Vue from 'vue';
  export default{
    name: 'cartcontrol',
    data () {
      return {

      }
    },
    props:{
      food:{
        type:Object
      }
    },
    created(){
    },
    methods:{
      addCart(event){
//        if(!event._contructed){
//          return;
//        }
        if(!this.food.count){
          Vue.set(this.food,'count',1);
        }else{
          this.food.count++;
        }
        console.log(this.food.count);
        this.$emit('cart.add',event.target);
      },
      decreaseCart(){
        if(this.food.count){
          this.food.count--;
        }
      }
    }
  }
</script>
<style scoped>
  .cartcontrol{
    font-size: 0;
  }
  .cart-decrease{
    display: inline-block;
    padding:6px;
    transition: all 0.4s linear;
  }
  .cart-decrease.move-transition{
    opacity: 1;
    transform: translate3D(0,0,0);
  }
  .cart-decrease.move-transition.move-enter,.cart-decrease.move-transition.move-leave{
    opacity: 0;
    transform: translate3D(24px,0,0);
  }
  .cart-decrease.move-transition.move-enter .inner,.cart-decrease.move-transition.move-leave .inner{
    transform: rotate(180deg);
  }
  .cart-decrease.move-enter,.cart-decrease.move-leave{
    opacity: 0;
    transform: translate3D(24px,0,0);
  }
  .cart-decrease .inner,.cart-decrease.move-transition .inner{
    display: inline-block;
    line-height: 12px;
    font-size: 12px;
    color:rgb(0,160,220);
    transition: all 0.4s linear;
    transform: rotate(0);
  }
  .cart-add{
    display: inline-block;
    padding:6px;
    line-height: 12px;
    font-size: 12px;
    color:rgb(0,160,220)
  }
  .cart-decrease{}
  .cart-count{
    display: inline-block;
    vertical-align: top;
    width:12px;
    padding-top:6px;
    /*line-height: 24px;*/
    text-align: center;
    font-size: 10px;
    color:rgb(147,153,159);
  }
  .cart-add{

  }
</style>
