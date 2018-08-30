<template>
    <div class="ratingWrapper" ref="ratings">
      <div class="ratingContent">
        <div class="ratingTop">
          <div class="averScore">
            <p class="scores">{{seller.score}}</p>
            <p class="zhpf">综合评分</p>
            <p class="higher">高于周边商家{{seller.rankRate}}%</p>
          </div>
          <div class="stars">
            <p>服务态度 <Star :score="seller.serviceScore" :size="24" class="star"/> <span class="TRFont">{{seller.serviceScore}}</span></p>
            <p>服务态度 <Star :score="seller.foodScore" :size="24" class="star"/> <span class="TRFont">{{seller.foodScore}}</span></p>
            <p>送达时间 <span class="star min">{{seller.deliveryTime}}分钟</span></p>
          </div>
        </div>
        <div class="ratingMain">
          <div class="buttonBox">
            <a class="all" @click="allClick">全部{{all}}</a>
            <a class="good" @click="goodClick">满意{{good}}</a>
            <a class="bad" @click="badClick">不满意{{bad}}</a>
          </div>
          <div class="smile"><i class="icon-smile"></i>只看有内容的评价</div>
          <div class="commendBox">
            <div class="commend" v-for="item in ratingsBasicData">
              <div class="avatar">
                <img :src="item.avatar" width="28" height="28" alt="">
              </div>
              <div class="commendContent">
                <div class="times">{{new Date(item.rateTime)}}</div>
                <div class="userName">{{item.username}}</div>
                <div class="userStar"><Star :size="24" :score="item.score"/><span class="userTime">{{item.deliveryTime}}分钟送达</span></div>
                <div class="userCommend">{{item.text}}</div>
                <div class="userZan" v-if="item.recommend.length>0">
                  <i class="icon-smile"></i><span class="zanItem" v-for="recoItem in item.recommend">{{recoItem}}</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
</template>
<script>
    import Star from '../star/star.vue'
    import BScroll from 'better-scroll'
    export default{
      name: 'ratings',
      data () {
        return {
          all:0,
          good:0,
          bad:0,
          ratingsBasicData:[]
        }
      },
      props:{
        seller:{
          type:Object
        },
        ratings:{
          type:Array
        }
      },
      components:{
        Star
      },
      methods:{
        _initScroll(){
          this.$nextTick(()=>{
            if(!this.ratScroll){
              this.ratScroll =new BScroll(this.$refs.ratings,{
                click:true
              });
            }else{
              this.ratScroll.refresh();
            }
          })
        },
        goodClick(){
          var goodData=[];
          for(let i= 0,len=this.ratings.length;i<len;i++){
            if(this.ratings[i].score>3){
              goodData.push(this.ratings[i].score)
            }
            this.ratingsBasicData=goodData;
          }
        },
        badClick(){
          var badData=[];
          for(let i= 0,len=this.ratings.length;i<len;i++){
            if(this.ratings[i].score<=3){
              badData.push(this.ratings[i].score)
            }
            this.ratingsBasicData=badData;
          }
        },
        allClick(){
          this.ratingsBasicData=this.ratings;
        }
      },
      created(){
        this.$nextTick(() => {
          this._initScroll();
        })
        for(let i= 0,len=this.ratings.length;i<len;i++){
          this.ratingsBasicData.push(this.ratings[i]);
          if(this.ratings[i].score>3){
            this.good++;
          }else{
            this.bad++;
          }
          this.all++;
        }
      }
    }
</script>
<style scoped>
  .ratingWrapper{
    position:absolute;
    top:174px;bottom:10px;
    width:100%;
    overflow: hidden;
  }
  .ratingTop{
    display: flex;
    padding:18px 0;
  }
  .averScore{
    text-align: center;
    padding:0 24px;
    border-right:1px solid rgba(7,17,27,0.1);
  }
  .averScore .scores{
    font-size: 24px;
    color:rgb(255,153,0);
    line-height: 28px;
    margin-top: 6px;
  }
  .averScore .zhpf{
    font-size: 12px;
    color:rgb(7,17,27);
    line-height: 12px;
    margin-top: 8px;
  }
  .averScore .higher{
    margin-top: 6px;
    font-size: 10px;
    color:rgb(7,17,27);
    line-height: 10px;
  }
  .stars{
    padding:0 24px;
    font-size: 12px;
    color:rgb(7,17,27);
    line-height: 18px;
  }
  .TRFont{
    font-size: 12px;
    line-height: 18px;
    color:rgb(255,153,0);
  }
  .stars .star{
    display: inline-block;
    margin: 0 12px;
  }
  .stars .star.min{
    color:rgb(147,153,159);
  }

  .ratingMain{
    padding:0 24px;
  }
  .buttonBox{
    padding:18px 0;
    border-bottom: 1px solid rgba(7,17,27,0.1);
  }
  .buttonBox a{
    display: inline-block;
    outline: none;
    padding:10px 10px;
    font-size: 12px;

  }
  .buttonBox a.all{
    background: #009ED9;
    color: #fff;
  }
  .buttonBox a.good{
    background: #D1EEFA;
  }
  .buttonBox a.bad{
    background: #EBEDEF;
  }

  .smile{
    padding:18px 0;
  }
  .smile i{
    margin-right: 10px;
  }
  /*评论*/
  .commendContent{
    width:100%;
    position: relative;
  }
  .times{
    position: absolute;
    right:0;top:0;
    color:rgb(147,153,159);
    font-size: 10px;
    font-weight: 100;
    line-height: 12px;
  }
  .commend{
    display: flex;
    padding:18px 0;
    border-bottom:1px solid rgba(7,17,27,0.1);
  }
  .commend:last-child{
    border:none;
  }
  .avatar{
    width:28px;height:28px;
    border-radius: 50%;
    overflow: hidden;
    margin-right:12px;
  }
  .userName{
    font-size: 10px;
    color:rgb(7,17,27);
    line-height: 12px;
  }
  .userStar{
    padding:4px 6px 6px 0;
  }
  .userTime{
    font-size: 10px;
    font-weight: 100;
    color:rgb(147,153,159);
    line-height: 12px;
  }
  .userCommend{
    font-size: 12px;
    color:rgb(7,17,27);
    line-height: 18px;
  }
  .userZan{}
  .userZan .icon-smile{
    font-size: 12px;
    color:rgb(0,160,220);
    line-height: 16px;
  }
  .userZan .zanItem{
    display: inline-block;
    border:1px solid rgba(7,17,27,0.1);
    margin:8px 4px 0 4px;
    padding:0 3px;
    font-size:9px;
    color:rgb(147,163,159);
    line-height: 18px;
  }
</style>
