<template>
    <div class="ratings" ref="ratingsweprre">
       <div class="ratings-content">
            <div class="rating-content">
            <div class="pj">评价</div>
            <div class="pj">满意</div>
            <div class="pj">不满意</div>
        </div>
        <div class="rating-wrapper">
            <ul>
                <li v-for="rating in seller">
                    <div>{{rating.username}}</div>
                    <div>{{rating.rateTime}}</div>
                    <div>{{rating.rateType}}</div>
                    <div>{{rating.text}}</div>
                </li>
            </ul>
        </div>
       </div>
        
    </div>
</template>

<script>
import BScroll from "better-scroll";
export default {
    data(){
        return{
            seller:[]
        }
    },
    created(){
        this.$http.get('/api/ratings').then((result) =>{
        //  console.log(result.body.data)
            var result = result.body.data;
            this.seller = result;
            this.$nextTick(()=>{
                this.scroll = new BScroll(this.$refs.ratingsweprre, {
                click:true
             })
            })
            
        })
    },
    components:{
    }
}
</script>

<style scoped>
 .ratings{
        position: absolute;
        top: 174px;
        bottom: 46px;
        width: 100%;
        overflow: hidden;
 }
 .rating-content .pj {
     float: left;
     width: 80px;
     height: 40px;
     line-height: 40px;
    text-align: center;
     background-color: skyblue;
     margin: 0 3px;
 }
 .rating-wrapper{
     margin-top: 50px;
 }
</style>
