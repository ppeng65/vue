<template>
  <div class="star-wrapper" :class="starType">
    <span class="star" v-for="itemClass in itemClasses" :class="itemClass"></span>
  </div>
</template>

<script type="text/ecmascript-6">
  const LENGTH = 5;
  const CLS_ON = 'on';
  const CLS_HALF = 'half';
  const CLS_OFF = 'off';

  export default {
    props: ['size', 'score'],
    computed: {
      starType() {
        return 'star-' + this.size;
      },
      itemClasses() {
        let result = [];
        let score = Math.floor(this.score * 2) / 2;
        let hasDecimal = score % 1 !== 1;
        let integer = Math.floor(score);
        for(let i = 0; i < integer; i++) {
          result.push('on');
        }
        if(hasDecimal) {
          result.push('half');
        }
        while(result.length < LENGTH) {
          result.push('off');
        }
        return result;
      }
    }
  };
</script>

<style lang="stylus" scoped>
  @import '../../common/stylus/bg';

  .star-wrapper
    .star
      display inline-block
      &:last-child
        margin-right 0
    &.star-48
      .star
        width .4rem
        height .4rem
        margin-right .44rem
        &.on
          bg-image(star48_on)
        &.half
          bg-image(star48_half)
        &.off
          bg-image(star48_off)
    &.star-36
      .star
        width .3rem
        height .3rem
        margin-right .12rem
        &.on
          bg-image(star36_on)
        &.half
          bg-image(star36_half)
        &.off
          bg-image(star36_off)
    &.star-24
      .star
        width .2rem
        height .2rem
        margin-right .06rem
        &.on
          bg-image(star24_on)
        &.half
          bg-image(star24_half)
        &.off
          bg-image(star24_off)
</style>
