<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="tab border-1px">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <keep-alive>
      <router-view :seller="seller"></router-view>
    </keep-alive>
  </div>
</template>
<script type='text/ecmascript-6'>
import header from 'components/header/header.vue';
import { urlParse } from 'common/js/util';
const ERR_OK = 0;
export default {
  data () {
    return {
      seller: {
        // id是从url参数中获取的，采用一个立即执行函数
        id: (() => {
          let queryParam = urlParse();
          return queryParam.id;
        })()
      }
    };
  },
  created () {
    this.$http.get('api/seller?id=' + this.seller.id).then((response) => {
      response = response.body;
      if (response.errno === ERR_OK) {
        this.seller = Object.assign({}, this.seller, response.data);
      }
    }, () => { });
  },
  components: {
    'v-header': header
  }
};
</script>
<style lang="stylus" rel="stylesheet/stylus">
@import './common/stylus/mixin.styl'
#app
  .tab
    display: flex
    width: 100%
    height: 40px
    line-height: 40px
    // 调用border-1px（）函数
    border-1px(rgba(7, 17, 27, 0.1))
    .tab-item
      flex: 1
      text-align: center
      &:first-child
        z-index: 12
      // 以下表示父元素下的a元素
      & > a
        display: block
        font-size: 14px
        color: rgb(77, 85, 93)
        // 在实例化路由时设置路由的linkActiveClass
        &.active
          color: rgb(240, 20, 20)
          border-bottom: 1px solid red
</style>
