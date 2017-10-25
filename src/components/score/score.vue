<template>
  <div class="star-wrapper" :class="classType">
    <span class="star" v-for="starClass in starClasses" :class="starClass"></span>
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
      classType() {
        return 'star' + this.size;
      },
      starClasses() {
        let result = [];
        let score = Math.floor(this.score * 2) / 2;
        let integer = Math.floor(score);
        let hasDecimal = score % 1 !== 0;
        for(let i = 0; i < integer; i++) {
          result.push(CLS_ON);
        }
        if(hasDecimal) {
          result.push(CLS_HALF);
        }
        while(result.length < LENGTH) {
          result.push(CLS_OFF);
        }
        return result;
      }
    }
  };
</script>

<style lang="stylus" scoped>
  @import '../../common/stylus/bg.styl';
  .star-wrapper
    font-size 0
    .star
      display: inline-block
    &.star48
      .star
        width .4rem
        height .4rem
        margin-right .44rem
        &:last-child
          margin-right 0
        &.on
          bg-image(star48_on)
        &.half
          bg-image(star48_half)
        &.off
          bg-image(star48_off)
    &.star36
      .star
        width .3rem
        height .3rem
        margin-right .12rem
        &:last-child
          margin-right 0
        &.on
          bg-image(star36_on)
        &.half
          bg-image(star36_half)
        &.off
          bg-image(star36_off)
    &.star24
      .star
        width .2rem
        height .2rem
        margin-right .06rem
        &:last-child
          margin-right 0
        &.on
          bg-image(star24_on)
        &.half
          bg-image(star24_half)
        &.off
          bg-image(star24_off)
</style>
