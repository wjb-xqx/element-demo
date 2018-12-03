<template>
   <div class="seller">
       <div class="seller-content" >
           <div class="overview">
               <h1>{{seller.name}}</h1>
               <div class="desc">
                   <span>{{seller.ratingCount}}</span>
                   <span>{{seller.sellerCount}}</span>
              
               {{seller.minPrice}}
                   {{seller.deliveryPrice}}
                 </div>
                <div class="prc-wrapper" ref='pic-wrapper'>
                     <ul class="pics" ref="pic-list">
                     <li v-for="item in seller.pics">
                         <img :src="item" alt="">
                     </li>
                 </ul>
                </div>
           </div>
       </div>
   </div>
</template>

<script>
import BScroll from "better-scroll";
export default {
    data(){
        return {
             
        }
    },
    props:{
        seller:{
            type:Object
        }
    },
    // ready(){
    //     if(this.seller.pics){
    //         let picWidth = 120;
    //         let margin = 6;
    //         let width = (picWidth + margin)+this.seller.pics.length -margin;
    //         this.$refs.picList.style.width = width + "px";
    //         this.$nextTick(()=>{
    //             this.picScroll = new BScroll(this.$refs.picWrapper,{
    //                 scrollY = true,
    //                 eventPassthrough:'vertical'
    //             })
    //         })
    //     }
    // },
    methods:{

    }
}
</script>

<style scoped>
   
</style>
