<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img width="64" height="64" :src="seller.avatar" alt="">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">{{seller.description}}/{{seller.deliveryTime}}分钟送达</div>
        <div v-if="seller.supports" class="support">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div v-if="seller.supports" class="support-count" @click="showDetail">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin-wrapper">
      <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
      <img :src="seller.avatar" alt="" width="100%" height="200%">
    </div>
    <transition name="fade">
      <div class="detail" v-show="detailShow">
        <div class="detail-wrapper">
          <div class="main">
            <div class="name">{{seller.name}}</div>
            <div class="score">
              <star :size="48" :score="seller.score"></star>
            </div>
            <div class="line-title">
              <div class="line"></div>
              <div class="text">优惠信息</div>
              <div class="line"></div>
            </div>
            <div class="supports">
              <div class="support-item" v-for="item in seller.supports">
                <span class="icon" :class="classMap[item.type]"></span>
                <span class="text">{{item.description}}</span>
              </div>

            </div>
            <div class="line-title">
              <div class="line"></div>
              <div class="text">商家公告</div>
              <div class="line"></div>
            </div>
            <div class="bulletin">
              <p class="text">{{seller.bulletin}}</p>
            </div>
          </div>
        </div>
        <div class="detail-close" @click="closeDetail">
          <i class="icon-close"></i>
        </div>
      </div>
    </transition>

  </div>
</template>

<script type="text/ecmascript-6">
  import star from './../star/star';

  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        detailShow: false
      };
    },
    methods: {
      showDetail() {
        this.detailShow = true;
      },
      closeDetail() {
        this.detailShow = false;
      }
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    components: {
      star
    }
  };

</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"
  .header
    position: relative
    color: #fff
    background: rgba(0, 0, 0, 0.2)
    overflow: hidden;
    .content-wrapper
      position: relative
      padding: 24px 12px 18px 24px
      font-size: 0
      .avatar
        display: inline-block
        vertical-align: top
        & > img
          border-radius: 2px
      .content
        margin-left 16px
        display: inline-block
        .title
          margin: 2px 0 8px 0
          .brand
            vertical-align: top
            display: inline-block
            width: 30px
            height: 18px
            bg-image('brand')
            background-size 30px 18px
            background-repeat: no-repeat
          .name
            vertical-align: top
            margin-left: 6px
            font-size: 16px
            line-height: 18px
            font-weight: bold

        .description
          margin-bottom: 12px
          line-height: 12px
          font-size: 12px
        .support
          .icon
            vertical-align: top
            display: inline-block
            width: 12px
            height: 12px
            margin-right: 4px
            background-size: 12px 12px
            background-repeat: no-repeat
            &.decrease
              bg-image('decrease_1')
            &.discount
              bg-image('discount_1')
            &.guarantee
              bg-image('guarantee_1')
            &.invoice
              bg-image('invoice_1')
            &.special
              bg-image('special_1')

          .text
            line-height: 12px
            font-size: 10px
      .support-count
        position: absolute
        right: 12px
        bottom: 18px
        padding: 0 8px
        height: 24px
        line-height: 24px
        border-radius: 14px
        background: rgba(0, 0, 0, 0.2)
        text-align: center
        .count
          vertical-align: top
          font-size: 10px
        .icon-keyboard_arrow_right
          line-height: 24px
          font-size: 10px

    .bulletin-wrapper
      position: relative
      height: 28px
      line-height: 28px
      padding: 0 22px 0 12px
      white-space: nowrap
      overflow: hidden
      text-overflow: ellipsis
      .bulletin-title
        display: inline-block
        vertical-align: top
        margin-top: 9px
        width: 22px
        height: 12px
        bg-image('bulletin')
        background-size: 22px 12px
        background-repeat: no-repeat
      .bulletin-text
        margin: 0 4px
        font-size: 10px
      .icon-keyboard_arrow_right
        position: absolute
        font-size: 10px
        right: 12px
        top: 9px
    .background
      position: absolute
      top: 0
      left: 0
      width: 100%
      height: 100%
      z-index: -1
      overflow: hidden
      filter: blur(5px)
    .fade-enter-active, .fade-leave-active {
      transition: opacity .5s
    }
    .fade-enter, .fade-leave-active {
      opacity: 0
    }
    .detail
      position: fixed
      display: flex
      flex-flow: column
      top: 0
      left: 0
      z-index: 100
      width: 100%
      height: 100%
      background: rgba(7, 17, 27, 0.8)
      backdrop-filter: blur(10px)
      overflow: auto
      .detail-wrapper
        flex: 1
        .main
          min-height: 100%
          margin-top: 64px
          .name
            font-size: 16px
            font-weight: 700
            line-height: 16px
            color: rgb(255, 255, 255)
            text-align: center
          .score
            margin-top: 16px
            margin-bottom: 28px
            text-align: center
          .line-title
            width: 80%
            margin: 0 auto
            display: flex
            .line
              position: relative
              top: -6px
              flex: 1
              border-bottom: 1px solid rgba(255, 255, 255, 0.2)
            .text
              font-size: 14px
              line-height: 14px
              font-weight: 700
              color: rgb(255, 255, 255)
              margin: 0 12px

          .supports
            width: 80%
            margin: 0 auto
            padding: 24px 0 28px
            .support-item
              font-size: 0
              margin: 0 12px
              margin-bottom: 12px
              &:last-child
                margin-bottom: 0
              .icon
                display: inline-block
                vertical-align: top
                margin-right: 6px
                width: 16px
                height: 16px
                background-size: 16px 16px
                &.decrease
                  bg-image('decrease_2')
                &.discount
                  bg-image('discount_2')
                &.guarantee
                  bg-image('guarantee_2')
                &.invoice
                  bg-image('invoice_2')
                &.special
                  bg-image('special_2')
              .text
                font-size: 12px
                line-height: 16px
                font-weight: 200
                color: #fff

          .bulletin
            width: 80%
            margin: 0 auto
            padding-top: 24px
            .text
              font-size: 12px
              line-height: 24px
              font-weight: 200
              color: #fff
              margin: 0 12px
      .detail-close
        height: 32px
        flex: 0
        text-align: center
        margin-bottom: 32px
        .icon-close
          font-size: 32px
</style>
