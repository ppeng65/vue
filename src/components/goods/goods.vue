<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuScroll">
      <ul>
        <li v-for="(good, index) in goods"  class="menu-item menu-item-hook" :class="{current: getIndex === index}" @click="handleClickIndex(index, $event)">
          <span class="menu-text border-bottom">
            <span class='meun-icon' :class="mapClass[good.type]" v-show="good.type > 0">{{good.icon}}</span>{{good.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodsScroll">
      <ul>
        <li class="foods-list foods-list-hook" v-for="good in goods">
          <h1 class="foods-name">{{good.name}}</h1>
          <ul>
            <li class="foods-item border-bottom" v-for="item in good.foods">
              <div class="foods-icon">
                <img :src="item.icon" alt="" width="100%" height="100%">
              </div>
              <div class="foods-content">
                <h3 class="title">{{item.name}}</h3>
                <p class="desc" v-show="item.description">{{item.description}}</p>
                <div class="extra">
                  <span class="sellCount">月售{{item.sellCount}}份</span>
                  <span class="rating">好评率{{item.rating}}</span>
                </div>
                <div class="price">
                  <span class="new">¥{{item.price}}</span>
                  <span class="old" v-show="item.oldPrice">¥{{item.oldPrice}}</span>
                </div>
                <div class="cartButton">
                  <cartbutton :food="item"></cartbutton>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <div class="cartshop">
      <cartshop :select-foods="selectFoods" :minPrice="seller.minPrice" :deliveryPrice="seller.deliveryPrice"></cartshop>
    </div>
  </div>
</template>

<script>
  import '../../common/stylus/border.styl';
  import BScroll from 'better-scroll';
  import cartshop from '../cartshop/cartshop';
  import cartbutton from '../cartbutton/cartbutton';

  const ERR_OK = 0;

  export default {
    data() {
      return {
        mapClass: [],
        goods: [],
        scrollY: 0,
        heightArr: []
      };
    },
    created() {
      this.mapClass = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
      this.$http.get('/api/goods').then(response => {
        if (response.body.errno === ERR_OK) {
          this.goods = response.body.data;
          this.$nextTick(() => {
            this._initScroll();
            this._getMenuHeight();
          });
        };
      }, response => {
        alert('数据未加载成功，请重新刷新');
      });
    },
    methods: {
      handleClickIndex(index, event) {
        if(!event._constructed) {
          return;
        }
        this.scrollY = this.heightArr[index];
        this.foodsScroll.scrollTo(0, -this.scrollY, 300);
      },
      _initScroll() {
        this.menuScroll = new BScroll(this.$refs.menuScroll, {
          click: true
        });

        this.foodsScroll = new BScroll(this.$refs.foodsScroll, {
          probeType: 3,
          click: true
        });
        this.foodsScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y));
        });
      },
      _getMenuHeight() {
        let el = this.$refs.foodsScroll.getElementsByClassName('foods-list-hook');
        let height = 0;
        this.heightArr.push(height);
        for(let i = 0; i < el.length; i++) {
          height += el[i].clientHeight;
          this.heightArr.push(height);
        }
      }
    },
    computed: {
      getIndex() {
        for(let i = 0; i < this.heightArr.length; i++) {
          let height1 = this.heightArr[i];
          let height2 = this.heightArr[i + 1];
          if(!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            return i;
          }
        }
        return 0;
      },
      selectFoods() {
        let foods = [];
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if(food.count) {
              foods.push(food);
            }
          });
        });
        return foods;
      }
    },
    components: {
      cartshop,
      cartbutton
    },
    props: {
      seller: {
        type: Object,
        default() {
          return {};
        }
      }
    }
  };
</script>

<style lang="stylus" scoped>
  @import '../../common/stylus/bg.styl';

  .goods
    position absolute
    top 3.48rem
    left 0
    right 0
    bottom .96rem
    display flex
    overflow hidden
    .menu-wrapper
      flex 0 0 1.6rem
      width 1.6rem
      background-color #f3f5f7
      .menu-item
        display table
        width 1.6rem
        height 1.08rem
        &:last-child
          .border-bottom
            &:before
              border none
        &.current
          position relative
          margin -.02rem 0
          font-weight 700
          background-color #fff
          .border-bottom
            &:before
              border none
        .menu-text
          display table-cell
          vertical-align middle
          padding 0 .24rem
          width 1.12rem
          height 1.08rem
          font-size .24rem
          line-height .28rem
          color rgb(7, 17, 27)
        .border-bottom
          &:berfore
            border-bottom-color rgba(7, 17, 27, .1)
          .meun-icon
            display inline-block
            vertical-align top
            width .24rem
            height .24rem
            &.decrease
              bg-image(decrease_3)
            &.discount
              bg-image(discount_3)
            &.guarantee
              bg-image(guarantee_3)
            &.invoice
              bg-image(invoice_3)
            &.special
              bg-image(special_3)
    .foods-wrapper
      flex 1
      background-color #fff
      .foods-list
        .foods-name
          padding-left .28rem
          height .52rem
          line-height .52rem
          font-size .24rem
          color rgb(147, 153, 159)
          background-color #f3f5f7
          border-left .04rem solid #d9dde1
        .foods-item
          display flex
          padding .36rem
          font-size 0
          &:last-child
            &:before
              border none
          .foods-icon
            flex 0 0 1.15rem;
            vertical-align top
            width 1.15rem
            height 1.15rem
          .foods-content
            flex 1
            padding .04rem 0 0 .2rem
            .title
              padding-bottom .16rem
              font-size .28rem
              line-height .28rem
              color rgb(7, 17, 27)
            .desc
              padding-bottom .16rem
              font-size .2rem
              color rgb(147, 153, 159)
              line-height .25rem
            .extra
              .sellCount
                display inline-block
                font-size .2rem
                color rgb(147, 153, 159)
                line-height .2rem
              .rating
                display inline-block
                margin-left .1rem
                font-size .2rem
                color rgb(147, 153, 159)
                line-height .2rem
            .price
              .new
                font-size .28rem
                font-weight 700
                line-height .48rem
                color rgb(240, 20, 20)
              .old
                margin-left .1rem
                font-size .2rem
                font-weight normal
                line-height .48rem
                color rgb(147, 153, 159)
                text-decoration line-through
            .cartButton
              position absolute
              right .2rem
              bottom .2rem
    .cartshop
      position fixed
      left 0
      bottom 0
      width 100%
      height .96rem
</style>
