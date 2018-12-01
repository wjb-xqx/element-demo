<template>
   <div class="goods">
       <div class="metu-wrapper" ref="metuWrapper">
           <ul>
               <li v-for="(item,index) in goods" class="metu-item" 
               :class="{'current' :currentIndex===index}" @click="selectMenu(index)">
                   <span>{{item.name}}</span>
               </li>
           </ul>
       </div>
       <div class="foods-wrapper" ref="foodsWrapper">
           <ul>
               <li v-for="item in goods" class="foot-list food-list-hook">
                    <h1 class="title">{{item.name}}</h1>
                    <ul>
                        <li  @click="selectFood(food,event)"
                         v-for="food in item.foods" class="foot-item" >
                                <div class="icon">
                                    <img :src="food.icon" alt="">
                                </div>
                                <div class="content">
                                    <h2>{{food.name}}</h2>
                                    <p>{{food.description}}</p>
                                    <div class="extra">
                                        <span>月售{{food.sellCount}}份</span>
                                        <span>好评率{{food.rating}}%</span>
                                    </div>
                                    <div class="price">
                                        <span>￥{{food.price}}</span>
                                        <span v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                                    </div>
                                    <div class="cartconyrol-wrapper" >
                                        <cartcontrol :food="food"></cartcontrol>
                                    </div>
                                </div>
                        </li>
                    </ul>
               </li>
           </ul>
       </div>
       <!-- 传递参数   :select-foods="selectFoods" 必须是用-隔开 -->
       <shopCart  :select-foods="selectFoods"  :selet="selectFoods" :delivery-price="seller.deliveryPrice" :minPrice="seller.minPrice"></shopCart>
   </div>
  
</template>

<script>
import BScroll from "better-scroll";
import shopCart from '../shopcart/shopcart.vue';
import cartcontrol from '../cartcontrol/cartcontrol.vue';

export default {
    props:['seller'],
     data(){
        return{
            goods:[],
            // 每个区间的高度
            listHeight:[],
            scrollY:0
        }
    },
    components: {
      shopCart,
      cartcontrol
    },
    computed:{
       currentIndex() {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i];
          let height2 = this.listHeight[i + 1];
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            return i;
          }
        }
        return 0;
      },
      //传递增加商品的个数传递给购物车
      selectFoods() {
        let foods = [];
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count) {
              foods.push(food);
            }
          });
        });
        return foods;
      }
    },
    created(){
        this.$http.get('/api/goods').then((result) =>{
            var result = result.body.data;
            this.goods = result;
            this.$nextTick(() => {
                this._initScroll();
                this._calulateHeight();
            })
           
        })
        // console.log(this.selectFoods)
    },
     methods: {
         //点击左侧跳转对应部分
          selectMenu(index) {
        let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
        let el = foodList[index];
        this.foodsScroll.scrollToElement(el, 300);
      },
      _initScroll(){
        this.menuScroll = new BScroll(this.$refs.metuWrapper, {
            click:true
        })
        this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
             click:true,//添加商品的时候允许点击事件
             probeType:3});
        this.foodsScroll.on('scroll',(pos) => {
            this.scrollY = Math.abs(Math.round(pos.y));
        })
      },
      _calulateHeight(){
        //   拿到每个li
          let foodList= this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
            let height = 0;
            this.listHeight.push(height);
            for(let i = 0;i<foodList.length; i++){
                let item = foodList[i];
                height +=item.clientHeight;
                this.listHeight.push(height);
            }

     }
    }
}
</script>

<style scoped>
    .goods{
        display: flex;
        position: absolute;
        top: 174px;
        bottom: 46px;
        width: 100%;
        overflow: hidden;
    }
     .goods .metu-wrapper{
         flex:  0 0 80px;
         width: 80px;
         background-color: #f3f5f7;
     }
    
     .metu-item{
         padding: 0 12px;
         display: table;
         /* 多行都可以垂直居中 */
         height: 54px;
         width: 56px;
         line-height: 14px;
         background-color: #f3f5f7;
          border-bottom: 1px solid #ccc;
     }
    .current{
        background-color: #fff;
        position: relative;
        margin-top: -1px;
        z-index: 10;
        font-weight: 700;
    }
      .metu-item span {
         display: table-cell;
          font-size: 12px;
           vertical-align: middle;
           width: 56px;
          margin-right: 2px;
          line-height: 14px;
          font-weight: 200;
        display: inline-block
      }
      .metu-item span .active {
          color: rgb(240, 20, 20)
      }
       .foods-wrapper {
         flex: 1;
     }
     .foot-item {
         position: relative;
         display: flex;
     }
     .icon {
         flex: 1;
        
     }
     .icon img {
         height: 87px;
     }
      .foot-list .title{
          padding: 0px 0px 0px 14px;
          font-size: 12px;
          color: rgb(147, 153, 159);
          line-height: 26px;
          height: 26px;
          background-color: rgba(7, 17, 27,0.1);
      }
      .foot-item {
            border-bottom: 1px solid #ccc;
            padding: 18px 0px 14px;
            margin-left: 14px;
      }
      .foot-item:last-child{
          border-bottom: 0;
      }
      .content {
          flex: 4;
          padding-left: 10px;
      }
      .content  h2{
          margin-top: 2px;
          font-size: 14px;
          color: rgb(7, 17, 27);
          line-height: 14px;
      }
      .content  >p,
       .extra >span {
          font-size: 10px;
          color: rgb(147, 153, 159);
          line-height: 10px;
      }
       .content  >p{
           padding: 8px 0px;
       }
       .price{
           display: inline-block;
       }
       .price span{
           font-size: 12px;
           color: rgb(147, 153, 159);
           line-height: 24px;
            padding-right: 8px;
       }
       .price span:first-child{
           color: red;
       }
       .price span:nth-child(2){
           text-decoration: line-through;
       }
       .cartconyrol-wrapper{
           display: inline-block;
           position: absolute;
            bottom: 21px;
            right: 16px;
       }
</style>
