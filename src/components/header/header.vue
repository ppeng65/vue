<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar-wrapper">
        <img class="avatar" :src="seller.avatar" alt="">
      </div>
      <div class="content">
        <div class="name-wrapper">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">{{seller.description}}/{{seller.deliveryTime}}分钟送达</div>
        <div class="supports-wrapper" v-if="seller.supports">
          <span class="supports-icon" :class="this.mapClass[seller.supports[0].type]"></span>
          <span class="supports">{{seller.supports[0].description}}</span>
        </div>
        <div class="icon-wrapper" v-if="seller.supports">
          <div class="text">{{seller.supports.length}}个</div>
          <div class="icon-keyboard_arrow_right"></div>
        </div>
      </div>
    </div>
    <div class="bulletin-wrapper">
      <span class="bulletin-icon"></span><span class="bulletin">{{seller.bulletin}}</span>
      <div class="icon-keyboard_arrow_right"></div>
    </div>
    <div class="bgImage">
      <img :src="seller.avatar" alt="">
    </div>
  </div>
</template>

<script>
  export default {
    props: ['seller'],
    created() {
      this.mapClass = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    }
  };
</script>

<style lang="stylus" scoped>
  @import '../../common/stylus/bg';
  .header
    position relative
    overflow hidden
    width 100%
    color #fff
    background-color rgba(0, 0, 0, .5)
    .content-wrapper
      position relative
      padding .48rem .24rem .36rem .48rem
      font-size 0
      .avatar-wrapper
        display inline-block
        overflow hidden
        vertical-align top
        width 1.28rem
        height 1.28rem
        .avatar
          width 100%
          border-radius .04rem
      .content
        display inline-block
        padding .04rem 0 .04rem .32rem
        .name-wrapper
          .brand
            display inline-block
            vertical-align top
            margin-right .12rem
            width .6rem
            height .36rem
            bg-image(brand)
          .name
            display inline-block
            font-size .32rem
            line-height .36rem
            font-weight bold
        .description
          margin .16rem 0 .2rem
          font-size .24rem
          line-height .24rem
          font-weight 200
        .supports-wrapper
          .supports-icon
            display inline-block
            vertical-align top
            margin-right .08rem
            width .24rem
            height .24rem
            &.decrease
              bg-image(decrease_1)
            &.discount
              bg-image(discount_1)
            &.guarantee
              bg-image(guarantee_1)
            &.invoice
              bg-image(invoice_1)
            &.special
              bg-image(special_1)
          .supports
            display inline-block
            font-size .2rem
            font-weight 200
            line-height .24rem
        .icon-wrapper
          position absolute
          right .24rem
          bottom .26rem
          padding .14rem .16rem
          background-color rgba(0, 0, 0, .2)
          border-radius .3rem
          .text
            display inline-block
            margin-right .04rem
            font-size .2rem
            line-height .24rem
            font-weight 200
          .icon-keyboard_arrow_right
            display inline-block
            font-size .2rem
            vertical-align top
            line-height .24rem
    .bulletin-wrapper
      position relative
      height 100%
      line-height .56rem
      padding 0 .44rem 0 .24rem
      white-space nowrap
      overflow hidden
      text-overflow ellipsis
      font-size .2rem
      background-color rgba(7, 17, 27, .2)
      font-weight 200
      .bulletin-icon
        display inline-block
        vertical-align top
        margin-top .16rem 
        width .44rem
        height .24rem
        bg-image(bulletin)
      .bulletin
        margin 0 .08rem
      .icon-keyboard_arrow_right
        position absolute
        right .24rem
        bottom .15rem
        font-size .2rem
    .bgImage
      position absolute
      top 0
      left 0
      z-index -1
      width 100%
      height 100%
      filter blur(10px)
      img 
        width 100%
</style>
