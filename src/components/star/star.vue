<template>
  <div class="star" :class="starType">
    <span class="star-item" v-for="item in starClass" :class="item"></span>
  </div>
</template>

<script type="text/ecmascript-6">
  const STAR_ON = 'on';
  const STAR_HALF = 'half';
  const STAR_OFF = 'off';
  const SATR_COUNT = 5;
  export default {
    props: {
      size: {
        type: Number
      },
      score: {
        type: Number
      }
    },
    computed: {
      starType() {
        return 'star-' + this.size;
      },
      starClass() {
        let count = Math.floor(this.score * 2) / 2;
        let intCount = Math.floor(count);
        let isDecimal = count % 1 !== 0;
        let starClass = [];
        for (let i = 0; i < intCount; i++) {
          starClass.push(STAR_ON);
        }
        if (isDecimal) {
          starClass.push(STAR_HALF);
        }
        while (starClass.length < SATR_COUNT) {
          starClass.push(STAR_OFF);
        }
        return starClass;
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import '../../common/stylus/mixin.styl';
  .star
    font-size: 0
    .star-item
      display: inline-block
      background-repeat: no-repeat
    &.star-48
      .star-item
        width: 30px
        height: 30px
        background-size: 30px 30px
        margin-right: 22px
        &:last-child
          margin-right: 0
        &.on
          bg-image('star48_on')
        &.half
          bg-image('star48_half')
        &.off
          bg-image('star48_off')
    &.star-36
      .star-item
        width: 15px
        height: 15px
        background-size: 15px 15px
        margin-right: 6px
        &:last-child
          margin-right: 0
        &.on
          bg-image('star36_on')
        &.half
          bg-image('star36_half')
        &.off
          bg-image('star36_off')
    &.star-24
      .star-item
        width: 10px
        height: 10px
        margin-right: 3px
        background-size: 10px 10px
        &:last-child
          margin-right: 0
        &.on
          bg-image('star24_on')
        &.half
          bg-image('star24_half')
        &.off
          bg-image('star24_off')

</style>
