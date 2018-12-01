<template>
    <div class="shopcart">
        <div class="content">
            <div class="content-left">
                <div class="logo-wrapper">
                    <div class="logo" :class="{'highlight':totalCount>0}"><span>&</span>
                    </div>
                    <div class="num" v-show="totalCount > 0">{{totalCount}}</div>
                </div>
                <div class="price" :class="{'highlight1':totalPrice > 0}">￥{{totalPrice}}</div>
                <div class="desc">另需配送费￥ {{deliveryPrice}} 元</div>
            </div>
            <div class="content-right" :class="patClass">
                <div class="pay">{{payDesc}}</div>
            </div>
        </div>
        
    </div>
</template>

<script>
export default {
    props:{
        //从goods中传来good数据
        selectFoods:{
            type:Array,
            default(){
                return{}
            }
        },
        deliveryPrice:{
            type:Number,
            default:0
        },
        minPrice:{
            type:Number,
            default:0
        }
    },
    created(){
        // console.log(this.selectFoods.price)
    },
    computed:{
        totalPrice(){
            //商品总价
            let total = 0;
            this.selectFoods.forEach((food) =>{
                total += food.price * food.count;
            });
            return total;
        },
        totalCount(){
            let count = 0;
            this.selectFoods.forEach((food)=>{
                count+=food.count;
            });
            return count;
        },
        //与总价相比配送费的变化
        payDesc(){
            if(this.totalPrice === 0 ){
                return `￥${this.minPrice} 元起送`;
            }else if(this.totalPrice<this.minPrice){
                let diff = this.minPrice - this.totalPrice
                return `还差￥${diff}元起送`
            }else{
                return `去结算`
            }
        },
        patClass(){
            if(this.totalPrice >=this.minPrice){
                return `enough`
            }else{
                return ``
            }
        }
    }
}
</script>

<style scoped>
.shopcart{
     position: fixed;
        bottom: 0;
        left: 0;
        width: 100%;
    }
    .content{
        display: flex;
        background-color: #141d27;
    }
    .content-left{
      display: flex;
      flex: 1;
    }
     .content-right{
        flex: 0 0 105px;
        width: 105px;
    }
    .logo-wrapper{
        width: 56px;
        height: 56px;
         background-color: #141d27;
        position: relative;
        top: -10px;
        margin-left: 14px;
        border-radius: 50%;
        text-align: center;
        padding: 3px;
        line-height: 40px;
    }
    .logo{
        width: 40px;
        height: 40px;
        background-color: rgba(255, 255, 255, 0.3);
          border-radius: 50%;
          margin-left: 8px;
          margin-top: 3px;
          color: #fff;
    }
    
    .highlight{
        background-color: rgb(0, 160, 220);
    }
    .num{
        position: absolute;
        top: 0;
        right: 0;
        width: 24px;
        height: 16px;
        line-height: 16px;
        text-align: center;
        border-radius: 50%;
        font-size: 9px;
        font-weight: 700;
        color: #fff;
        background-color: red;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.4);

    }
    .price{
        display: inline-block;
        line-height: 24px;
        margin: 14px 5px;
        padding-right: 12px;
        box-sizing: border-box;
        border-right: 1px solid #ccc;
        font-size: 16px;
        color: #fff;
    }
    
    .highlight1{
        color: red;
        font-size: 18px;
    }
    .desc{
          display: inline-block;
        line-height: 24px;
        margin: 14px 5px;
        padding-right: 12px;
        font-size: 16px;
        color: #fff;
    }
    .content-right{
        background-color: rgba(255, 255, 255, 0.4);
        line-height: 24px;
        font-size: 16px;

    }
    
    .enough{
        background-color: #00b430;
    }
    .pay{
        height: 48px;
        line-height: 48px;
        text-align: center;
        font-size: 12px;
        color: #fff;
    }
    .ball-container{

    }
    .ball {
        position: fixed;
        left: 32px;
        bottom: 22px;
        z-index: 200;
    }
    
</style>
