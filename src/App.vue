<template>
  <div id="app">
    <div class="header">
      <v-header :seller="seller"></v-header>
    </div>
    <div class="tab border-1px">
      <router-link class="tab-item" to="/goods">商品</router-link>
      <router-link class="tab-item" to="/ratings">評論</router-link>
      <router-link class="tab-item" to="/seller">商家</router-link>
    </div>
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script>
  import header from './components/header/header.vue';
  import axios from 'axios';
  const ERR_OK = 0;

  export default {
    name: 'app',
    data() {
      return {
        seller: {}
      };
    },
    created() {
      axios({
        method: 'get',
        url: '/api/seller'
      })
        .then(response => {
          if (response.data.erron === ERR_OK) {
            this.seller = response.data.data;
//            console.log(this.seller);
          }
        })
        .catch(error => {
          console.log(error);
          alert('网络错误，不能访问');
        });
    },
    components: {
      'v-header': header
    },
    methods: {},
    watch: {}
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "./common/stylus/mixin.styl"
  #app
    .tab
      display: flex
      width: 100%
      height: 40px
      line-height: 40px
      border-1px (rgba(7, 17, 27, 0.3))
      .tab-item
        flex: 1
        text-align: center
        font-size: 14px
        color: rgb(77, 85, 93)
      .active
        font-size: 14px
        color: rgb(240, 20, 20)
</style>
