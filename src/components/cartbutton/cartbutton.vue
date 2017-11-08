<template>
  <div class="cartbutton">
    <transition name="move">
      <div class="reduce" v-show="food.count > 0" @click="handleClickReduce">
        <span class="inner icon-remove_circle_outline"></span>
      </div>\
    </transition>
    <div class="num" v-show="food.count > 0">{{food.count}}</div>
    <div class="add icon-add_circle" @click="handleClickAdd"></div>
  </div>
</template>

<script>
  import '@/common/stylus/icon.styl';
  import Vue from 'vue';
  export default {
    methods: {
      handleClickReduce(event) {
        if(!event._constructed) {
          return;
        }
        if(this.food.count) {
          this.food.count --;
        }
      },
      handleClickAdd(event) {
        if(!event._constructed) {
          return;
        }
        if(!this.food.count) {
          Vue.set(this.food, 'count', 1);
        }else {
          this.food.count ++;
        }
      }
    },
    props: {
      food: {
        type: Object
      }
    }
  };
</script>

<style lang="stylus" scoped>
  .cartbutton
    font-size 0
    .reduce
      vertical-align top
      display inline-block
      &.move-enter-active, &.move-leave-active
        opacity 1
        transition all .4s linear
      &.move-enter, &.move-leave-to
        opacity 0
        transform translate3d(48px, 0, 0)
      .inner
        display inline-block
        line-height .24rem
        font-size .2rem
        padding .24rem
        color #00a0dc
        font-size .48rem
    .num
      display inline-block
      margin-top .22rem
      color: rgb(147, 153, 159);
      font-size .2rem
      line-height .24rem
    .add
      vertical-align top
      display inline-block
      line-height .24rem
      font-size .2rem
      padding .24rem
      color #00a0dc
      font-size .48rem
</style>
