<template>
  <div>
    <v-header :seller='seller'></v-header>
    <div class="tab">
      <router-link to="/goods" tag="div" class="tab-item">商品</router-link>
      <router-link to="/ratings" tag="div" class="tab-item">评价</router-link>
      <router-link to="/seller" tag="div" class="tab-item">商家</router-link>
    </div>
    <router-view/>
  </div>
</template>

<script>
  import Header from '@/components/header/header';
  import './common/stylus/icon.styl';
  const ERR_OK = 0;

  export default {
    components: {
      'v-header': Header
    },
    data() {
      return {
        seller: {}
      };
    },
    created() {
      this.$http.get('/api/seller').then(response => {
        if (response.body.errno === ERR_OK) {
          this.seller = response.body.data;
        };
      }, response => {
        alert('数据未加载成功，请重新刷新');
      });
    }
  };
</script>

<style lang="stylus" scoped>
  .tab
    display: flex
    .tab-item
      flex: 1
      text-align: center
      line-height: .8rem
      color: rgb(77, 85, 93)
      font-size: .28rem
      &.router-link-active
        color: rgb(240, 20, 20)
</style>
