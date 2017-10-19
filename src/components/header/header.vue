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
          <span class="supports-icon" :class="mapClass[seller.supports[0].type]"></span>
          <span class="supports">{{seller.supports[0].description}}</span>
        </div>
        <div class="icon-wrapper" v-if="seller.supports" @click="handleClickShowDetail">
          <div class="text">{{seller.supports.length}}个</div>
          <div class="icon-keyboard_arrow_right"></div>
        </div>
      </div>
    </div>
    <div class="bulletin-wrapper" @click="handleClickShowDetail">
      <span class="bulletin-icon"></span><span class="bulletin">{{seller.bulletin}}</span>
      <div class="icon-keyboard_arrow_right"></div>
    </div>
    <div class="bgImage">
      <img :src="seller.avatar" alt="">
    </div>
    <div class="detail-wrapper" v-show="showDetail">
      <div class="detail">
        <div class="detail-content">
          <h1 class="name">{{seller.name}}</h1>
          <div class="star">
            <star :size="48" :score="seller.score"></star>
          </div>
          <div class="title">
            <span class="line"></span>
            <span class="text">优惠信息</span>
            <span class="line"></span>
          </div>
          <ul class="supports"  v-if="seller.supports">
            <li class="supports-item" v-for="item in seller.supports">
              <span class="icon" :class="mapClass[item.type]"></span>
              <span class="text">{{item.description}}</span>
            </li>
          </ul>
          <div class="title">
            <span class="line"></span>
            <span class="text">商家公告</span>
            <span class="line"></span>
          </div>
          <div class="seller-text">
            {{seller.bulletin}}
          </div>
        </div>
      </div>
      <div class="footer">
        <i class="icon-close" @click="handleCLickCloseDetail"></i>
      </div>
    </div>
  </div>
</template>

<script>
  import star from '../score/score';

  export default {
    props: ['seller'],
    created() {
      this.mapClass = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    data() {
      return {
        mapClass: [],
        showDetail: false
      };
    },
    methods: {
      handleClickShowDetail() {
        this.showDetail = true;
      },
      handleCLickCloseDetail() {
        this.showDetail = false;
      }
    },
    components: {
      star
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
    .detail-wrapper
      position fixed
      top 0
      left 0
      z-index 10
      overflow auto
      width 100%
      height 100%
      background-color rgba(7, 17, 27, .8)
      .detail
        min-height 100%
        .detail-content
          width 100%
          padding 1.28rem 0 1.92rem
          .name
            text-align center
            font-size .32rem
            line-height .32rem
            font-weight 700
          .star
            margin-top .32rem
            text-align center
          .title
            display flex
            align-items center
            width 80%
            margin .56rem auto .48rem auto
            span
              flex 1
              &.text
                padding 0 .24rem
                text-align center
              &.line
                height .02rem
                background-color rgba(255, 255, 255, .2)
          .supports
            width 75%
            margin 0 auto
            .supports-item
              margin-bottom .24rem
              font-size 0
              &:last-child
                margin-bottom 0
              .icon
                display inline-block
                vertical-align top
                margin-right .12rem
                width .32rem
                height .32rem
                &.decrease
                  bg-image(decrease_2)
                &.discount
                  bg-image(discount_2)
                &.guarantee
                  bg-image(guarantee_2)
                &.invoice
                  bg-image(invoice_2)
                &.special
                  bg-image(special_2)
              .text
                display inline-block
                font-size .24rem
                font-weight 200
                line-height .32rem
          .seller-text
            width 80%
            margin 0 auto
            padding 0 .24rem
            box-sizing border-box
            font-size .24rem
            line-height .48rem
            font-weight 200
      .footer
        position relative
        margin-top -1.92rem
        width 100%
        height 1.92rem
        text-align center
        .icon-close
          font-size .64rem
          line-height 1.92rem
          color rgba(0, 0, 0, .5)
</style>
