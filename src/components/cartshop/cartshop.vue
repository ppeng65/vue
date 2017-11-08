<template>
  <div class="cartshop">
    <div class="content-wrapper">
      <div class="content-left">
        <div class="cart-icon">
          <i class="icon-shopping_cart" :class="{highlight: foodsCount>0}"></i>
          <div class="count" v-show="foodsCount > 0">{{foodsCount}}</div>
        </div>
        <div class="pay" :class="{highlight: foodsCount>0}">¥{{totalPrice}}</div>
        <div class="deliveryPrice">另需配送费¥{{deliveryPrice}}元</div>
      </div>
      <div class="content-right" :class="priceClass">
        <div class="minPrice">{{diffPrice}}</div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    props: {
      minPrice: {
        type: Number
      },
      deliveryPrice: {
        type: Number
      },
      selectFoods: {
        type: Object,
        default() {
          return [];
        }
      }
    },
    computed: {
      foodsCount() {
        let count = 0;
        this.selectFoods.forEach((food) => {
          count += food.count;
        });
        return count;
      },
      totalPrice() {
        let total = 0;
        this.selectFoods.forEach((food) => {
          total += food.price * food.count;
        });
        return total;
      },
      diffPrice() {
        let diff = this.minPrice - this.totalPrice;
        if(this.totalPrice === 0) {
          return `¥${this.minPrice}元起送`;
        }else if(this.totalPrice < this.minPrice) {
          return `¥${diff}元起送`;
        }else {
          return '去结算';
        }
      },
      priceClass() {
        if(this.totalPrice < this.minPrice) {
          return 'no-enough';
        }else {
          return 'enough';
        }
      }
    }
  };
</script>

<style lang="stylus" scoped>
  .cartshop
    width 100%
    height 100%
    color #fff
    background-color #141d27
    .content-wrapper
      display flex
      font-size 0
      .content-left
        flex 1
        .cart-icon
          position relative
          float left
          padding .12rem
          margin -.16rem .3rem 0
          width .88rem
          height .88rem
          line-height .88rem
          text-align center
          background-color #141d27
          border-radius 100%
          .icon-shopping_cart
            display block
            width .88rem
            height .88rem
            line-height .88rem
            font-size .48rem
            border-radius 100%
            color #808589
            background-color #2b343d
            &.highlight
              color #fff
              background-color #00a0dc
          .count
            position absolute
            top 0
            right 0
            width .24rem
            padding 0 .12rem
            font-size .18rem
            font-weight 700
            line-height .32rem
            border-radius .14rem
            color #ffffff
            background-color rgb(240, 20, 20)
            box-shadow 0 4px 8px 0 rgba(0, 0, 0, .4)
        .pay
          display inline-block
          margin .24rem 0
          padding-right .2rem
          font-size .32rem
          line-height .48rem
          font-weight 700
          color rgba(255, 255, 255, .4)
          border-right .02rem solid rgba(255, 255, 255 , .1)
          &.highlight
            color #fff
        .deliveryPrice
          display inline-block
          margin-left .2rem
          font-size .24rem
          font-weight 300
          line-height .48rem
          color rgba(255, 255, 255, .4)
      .content-right
        width 2.1rem
        flex 0 0 2.1rem
        &.no-enough
          color #939498
          background-color #2b343b
        &.enough
          color #fff
          background-color #00a0dc
        .minPrice
          text-align center
          line-height .96rem
          font-size .24rem
          font-weight 700


</style>
