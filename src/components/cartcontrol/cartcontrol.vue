<template>
    <div class="cartcontrol">
         <!-- v-show="food.count>0" -->
         <transition>
              <div class="cart-decrease" v-show="food.count>0" @click.stop.prevent="decreadeCart">-</div>
              
         </transition>
           <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
        <div class="cart-add" @click.stop.prevent="addcart">+</div>
    </div>
</template>

<script>
import Vue from 'vue';
export default {
    props:{
        food:{
            type:Object
        }
    },
    methods:{
        addcart(){
            console.log('click')
            if(!this.food.count){
                // this.food.count =1 因为英文名还没有值这样是不行的，所以利用vue
                Vue.set(this.food,'count',1)
            }else{
                this.food.count++
            }
            
        },
    decreadeCart(){
        if (this.food.count) {
          this.food.count--;
        }
     }
    }
}
</script>

<style scoped>
.v-enter,
.v-leave-to {
opacity: 0;
transform: translate3D(100px,0,0,);
transform: rotate(180deg);
}

.v-enter-active,
.v-leave-active {
transition: all 0.4s linear;
}
.cartcontrol{
   /* padding-left: 100px; */
    display: inline-block;
    
}
    .cart-decrease{
        display: inline-block;
       padding: 2px;
      background-color: rgb(0, 160, 220);
      border-radius: 50%;
      color: #fff;
    }
    .cart-count{
         display: inline-block;
         
    }
    .cart-add{
       display: inline-block; 
      padding: 2px;
      background-color: rgb(0, 160, 220);
      border-radius: 50%;
      color: #fff;
    }
</style>
