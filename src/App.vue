<template>
  <div id="app">
      <Vheader ></vheader>
      <div class="v-nav">
       <div class="v-itme"> <router-link to="/api/goods" class="active">商品</router-link></div>
       <div class="v-itme"> <router-link to="/ratings" >评价</router-link></div>
       <div class="v-itme"> <router-link to="/seller" >商家</router-link></div>
      </div>
      <router-view></router-view>
      <div class="v-footer">footer</div>
  </div>
</template>

<script>
import Vheader from './components/header/header.vue'

export default {
  name: 'App',
  components:{
    Vheader
  }
}
</script>

<style >
.v-nav{
  display: flex;
  height: 40px;
  line-height: 40px;
   
}
.v-itme{
  flex: 1;
  border-bottom: 1px  solid  rgba(7, 12, 27, 0.1)
}
.v-itme a{
  display: block;
  text-align: center;
  font-size: 14px;
  color: rgb(77, 85, 93);
}
.v-itme a.active{
  color: rgb(240, 20, 20);
}
</style>
