<template>
    <div class="food" v-show="showFlag" ref="foodWrapper">
         <div class="food-content">
             <div class="img-header">
                 <img :src="food.icon" alt="">
             </div>
            <div class="info">
                 <h1 class="foodname">{{food.name}}</h1>
              <div class="foodrating">
                 <span>月售{{food.sellCount}}份</span>
                     <span>好评率{{food.rating}}%</span>
                </div>
            <div class="foodPrice">
                <span>￥{{food.price}}</span>
                <span>{{food.oldPrice}}</span>
            </div>
            </div>
            <div class="colro"></div>
             <div class="js">
                 <h2 class="">商品介绍</h2>
             <div class="content">
                 {{food.info}}
             </div>
             </div>
              <div class="colro"></div> 
             <div class="pj">
                <h1>商品评价</h1>
                <!-- <ratingselect :ratings="food.ratings"></ratingselect> -->
             </div>
         </div>
    </div>
</template>

<script>
import BScroll from "better-scroll";
import ratingselect from '../ratingselect/ratingselect.vue'
const SATISFY=0; //推荐
const UNSATISFY=1; //吐槽
const ALL = 2;
export default {
  props:{
       food:{
            type:Object
        }
  },
  components:{
      ratingselect
  },
  data(){
      return {
          showFlag :false,//显示或隐藏改组件
      }    
  },
  methods:{
      show(){
          this.showFlag =true;
            this.seletType = ALL;
         this.onlyContent = true;
         this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.foodWrapper, {
              click: true
            });
          } else {
            this.scroll.refresh();
          }
        });
  }
}
}
</script>

<style scoped>
    .food{
        position :fixed;
        left :0;
        top:0;
        bottom :71px;
        z-index:30;
        width :100%;
        background-color:#fff;
        overflow: hidden;
    }
    .img-header{
        width: 100%;
        height: 360px;
    }
    img {
        width: 100%;
        height: 100%;
    }
    .foodname{
        margin: 18px 0px 8px 18px;
        font-size: 18px;
        font-weight: 700;
        color:rgb(7, 17, 27);
        line-height: 14px;
    }
   
    .foodrating{
        color: rgb(147, 153, 159);
        line-height: 10px;
        margin-left: 18px;
    }
    .foodrating span:nth-child(2) {
        padding-left: 12px;
        
    }
     .foodPrice {
         margin: 18px 0px;
          margin-left: 18px;
     }
    .foodPrice span:first-child {
        color: red;
        line-height: 24px;
        font-size: 14px;
    }
    .foodPrice span:nth-child(2){
        color:rgb(147, 153, 159);
        line-height: 24px;
        font-size: 14px;
    }
    .colro{
        width: 100%;
        height: 16px;
        background-color: #f3f5f7;
    }
    .js{
        margin: 18px 0px 6px 18px;
    }
     .js h2 {
         color: rgba(255, 255, 255,0.7);
         font-size: 18px;
     }
    .js .content {
        font-size: 14px;
        color: rgb(77, 85, 93);
        line-height: 24px;
    }
    .pj{
        
    }
    .pj div{
       float: left;
       width: 100px;
       height: 32px;
       text-align: center;
    }
    .pj div:first-child{
        background-color: skyblue;
        color: #fff;
        line-height: 32px;
        border-radius: 2px;
    }
.pj div:nth-child(2){
    color: rgb(77, 85, 93);
    background-color:rgba(0, 160, 220, 0.2);
    line-height: 32px;
     border-radius: 2px;
     margin: 0px 8px;
}
.pj div:nth-child(3){
    color: rgb(77, 85, 93);
    background-color:rgba(77, 85, 93, 0.2);
    line-height: 32px;
     border-radius: 2px;
}
</style>
