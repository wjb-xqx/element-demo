<template>
    <div class="ratingselect">
        <div class="rating-type">
            <!-- <span>{{desc.all}}</span>
            <span>{{desc.positive}}</span>
            <span>{{desc.negative}}</span> -->
           <ul>
               <ll v-for="(user,i) in ratings" :key="i">
                   <span>{{user.username}}</span>
               </ll>
           </ul>
        </div>
        <div class="switch">
            <span class="text">只看内容的评价</span>
            
        </div>
    </div>
</template>

<script>
const SATISFY=0; //推荐
const UNSATISFY=1; //吐槽
const ALL=2; //全部
export default {
    props: {
        ratings:{
            type:Array
        }
    },
    created(){
        console.log(this.ratings)
    }
}
</script>

<style>

</style>
