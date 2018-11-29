
<template>
    <div class="herder">
        <div class="content">
            <div class="avatar">
              <img :src="seller.avatar" alt="" width="62" height="62">
            </div>
            <div class="info">
                <div class="title">
                    <img class="band" src="../../assets/img/brand@2x.png" alt="">
                    <span class="name">{{seller.name}}</span>
                </div>
                <div class="description">
                    {{seller.description}}/{{seller.deliveryTime}}
                </div>
                <div v-if="seller.supports" class="supports">
                    <img src="../../assets/img/decrease_1@2x.png"  class="icon" alt="">
                <span class="text">{{seller.supports[0].description}}</span>
                </div>
                 <div class="supports-count" v-if="seller.supports">
                <span class="count">{{seller.supports.length}}</span>
                <i class="icon-keyboard_arrow_right"></i>
            </div>
            </div>
        </div>
        <div class="bulletin" @click="sgowDetail">
          <!-- <img src="../../assets/img/decrease_1@2x" class="bulletin-title" alt=""> -->
          <div class="bulletin-text">{{seller.bulletin}}</div>
          <i class="icon-keyboard_arrow_right"></i>
        </div>
        <div class="background">
           <img :src="seller.avatar" alt="" width="100%" height="100%">
        </div>
        <div class="detail" v-show="detailShow">
            <div class="detail-swapper clearfix">
                <div class="detail-main">
                   <h1 class="d_name">{{seller.name}}</h1>
                   

                </div>
            </div>
            <span class="del" @click="updateil">X</span>
        </div>
    </div>
</template>

<script>
export default {
    data(){
        return{
            seller:[],
            detailShow:false
        }
    },
    created(){
        this.$http.get('/api/seller').then((result) =>{
            // seller=result.body;
            var result = result.body.data;
            this.seller = result
        })
    },
    methods:{
        sgowDetail(){
            this.detailShow = true;
        },
        updateil(){
            this.detailShow = false;
        }
    }
}
</script>

<style scoped>
/* 弹出层 */
.detail{
    position: fixed;
    z-index: 8;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    background-color:   rgba(7, 17, 27, 0.8);
    overflow :auto;
}
.detail-swapper{
    min-width: 100%;
}
.detail-swapper .detail-main{
    margin-top: 64px;
    padding-bottom: 64px;
    min-width: 100%;
    color:#fff;
}
.d_name{
    font-size: 16px;
    font-weight: 700;
    line-height: 16px;
    text-align: center;
}
.del{
    display: block;
    color: #fff;
   position: relative;
   width: 32px;
   height: 32px;
   margin: -64px auto 0 auto;
   clear: both;
   font-size: 32px;

}
    .herder{
        overflow: hidden;
        position: relative;
        display: flex;
        flex-wrap: wrap;
        background-color: rgba(0, 0, 0, 0.5);
        padding: 24px  0px 0px;
    }
    .content{
        display: flex;
       width: 100%;
       
    }
    .avatar{
        padding: 0px 8px 18px;

    }
    .avatar img{
        border-radius: 4px;
    }
    .title{
        line-height: 18px;
        display: block;
    }
    .title .band {
        width: 28px;
        height: 100%;
        vertical-align: middle;
    }
    .supports .icon{
        margin-bottom: 0;
        vertical-align: middle;
        width: 14px;
        height: 14px;
    }
    .name{
        font-size: 16px;
        color: rgb(255, 255, 255);
        font-weight: bold;
        line-height: 18px;
    }
    .description{
        font-size: 12px;
        color: rgb(255,255,255);
        font-weight: 200;
        line-height: 12px;
        padding: 4px 0;
    }
    .info{
           flex: 1;
           padding: 0px 0px 18px;
           position: relative;
    }
    .brand{
        background: url('../../assets/img/brand@2x.png');
    }
    .text{
        font-size: 10px;
        color: rgb(255, 255, 255);
        font-weight: 200;
        line-height: 12x;
    }
    .supports-count{
        position: absolute;
        right: 12px;
        bottom: 18px;
        padding: 0 8px;
        height: 24px;
        line-height: 24px;
        border-radius: 14px;
        background-color:rgba(0, 0, 0,0.2);
        color: rgb(255, 255, 255);
        font-size: 10px;
        
    }
    .icon-keyboard_arrow_right{
         font-size: 10px;
         padding: 2px;
         color: rgb(255, 255, 255);
    }
       .bulletin{
        background-color: rgba(2, 17, 27, 0.2);
        height: 28px;
        line-height: 28px;
        font-size: 10px;
        color: rgb(255, 255, 255);
        font-weight: 200;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
        padding: 0 22px 0 12px;
    }
    .background{
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        z-index: -1;
        filter: blur(10px) 
        /* 模糊效果 */
    }
</style>
