<template>
  <div id="app">
    <Header :seller="seller"/>
    <div class="tab border-1px">
      <div class="tab-item">
        <router-link :to="{path:'/goods'}">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link :to="{path:'/ratings'}">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link :to="{path:'/seller'}">商家</router-link>
      </div>
    </div>

    <router-view :seller="seller" :ratings="ratings"/>
  </div>
</template>

<script>
  import Header from './components/header/header'
  export default {
    name: 'App',
    data(){
      return{
        seller:{},
        ratings:[]
      }
    },
    components:{
      Header
    },
    created(){
      this.axios.get('../static/data.json').then((response) => {
        console.log(response);
        if(response.statusText=='OK'){
          this.seller=response.data.seller;
          this.ratings=response.data.ratings;
        }else{
          console.log('数据请求失败！')
        }
      })
    }
  }
</script>

<style>
  .tab{
    display: flex;
    width: 100%;
    height: 40px;
    line-height: 40px;
    /*border-bottom: 1px solid rgba(7, 17, 27, 0.1);*/
  }
  .tab-item{
    flex: 1;
    text-align: center;
  }
  .tab-item a{
    display: block;
    font-size: 14px;
    color: rgb(77, 85, 93);
    text-decoration: none;
  }
  .tab-item a:active{
    color: rgb(240, 20, 20);
  }
</style>
