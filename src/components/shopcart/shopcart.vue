<template>
    <div class="shopcart">
        <div class="content" @click="toggleList">
            <div class="content-left">
                <div class="logo-wrapper">
                    <div class="logo" :class="{'highlight':totalCount>0}"><span>&</span>
                    </div>
                    <div class="num" v-show="totalCount > 0">{{totalCount}}</div>
                </div>
                <div class="price" :class="{'highlight1':totalPrice > 0}">￥{{totalPrice}}</div>
                <div class="desc">另需配送费￥ {{deliveryPrice}} 元</div>
            </div>
            <div class="content-right" :class="patClass" @click.stop.prevent="pay">
                <div class="pay">{{payDesc}}</div>
            </div>
            <transition name="fold">
                <div class="shopcart-list"  v-show="listShow" >
                <div class="list-header">
                    <h1 class="title">购物车</h1>
                    <span class="empty" @click="empty">清空</span>
                </div>
                <div class="list-content" ref="listContent">
                    <ul>
                         <li class="food" v-for="food in selectFoods">
                             <span class="name">{{food.name}}</span>
                            <div class="price">
                                <span>￥{{food.price*food.count}}</span>
                            </div>
                            <div class="carcontrol-wrapper">
                                <cartcontrol :food="food"></cartcontrol>
                            </div>
                         </li>
                     </ul>   
                </div>
            </div>
            </transition> 
           
        </div>
    </div>  
    
</template>
 <!-- <transition name="mask">
             <div class="list-mask" v-show="listShow"></div>
    </transition> -->

<script>
import BScroll from "better-scroll";
import cartcontrol from '../cartcontrol/cartcontrol.vue';
export default {
    data(){
        return {
            fold:true //购物详情页
        }
    },
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
        },
        listShow(){
            if(!this.totalCount){
                this.fold=true;
                return false;
            }
            let show = !this.fold;
            if(show){
                this.$nextTick( ()=>{
                    if(!this.scroll){
                    this.scroll = new BScroll(this.$refs.listContent,{
                        click:true
                    });
                    }else{
                        this.scroll.refresh();
                    }
                })
            }
             return show;
        }
    },
    components:{
        cartcontrol
    },
    methods:{
        toggleList(){
            if(!this.totalCount){
                return;
            }
            this.fold = !this.fold;
        },
        empty(){
            this.selectFoods.forEach( (food) => {
                food.count = 0;
            })
        },
        pay(){
            if(this.totalPrice <this.minPrice){
                return;
            }
            window.alert(`支付${this.totalPrice}元`)
        }
    }
}
</script>

<style scoped>
.list-mask{
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 40;
 
}
.mask-enter-active, .mask-leave-active {
    opacity: 1;
    background-color: rgba(7, 17, 27, 0.6);
   
}
.mask-enter, .mask-leave-active {
    opacity: 0;
    background-color: rgba(7, 17, 27, 0);
}
/* 购物车详情页 */
.shopcart-list {
    position: absolute;
    top:0;
    left: 0;
    z-index: -1;
    width: 100%;
     transform: translate3d(0, -100%, 0);
    background-color: red;
}
.fold-enter-active, .fold-leave-active {
    transition: all .5s;
   
}
.fold-enter, .fold-leave-active {
    transform: translate3d(0, 0, 0);
}

.list-header{
    height: 40px;
    background-color: #ccc;
    padding: 0 18px;
    line-height: 40px;
    border-bottom: 1px solid rgba(7, 17, 27, 0.1);
}
.title{
    float: left;
    font-size: 14px;
    color: rgb(7, 17, 27);
    line-height: 40px;
}
.food{
    border-bottom: 1px solid #ccc;
}
.empty{
    float: right;
    font-size: 12px;
    color:rgb(0, 160, 220);
    line-height: 40px;
}
.content{
    padding: 3px;
}
.price{
    color: red;
    line-height: 24px;
}
.price span {
    color: red;
}
.carcontrol-wrapper{
    float: right;
     margin-top: 15px;
    line-height: 24px;
    
}
.list-content{
    padding:  0 18px;
    max-height: 217px;
    background-color: #fff;
    overflow: hidden;
}
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
