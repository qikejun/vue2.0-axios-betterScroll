<template>
  <div class="shopcart">
    <div class="content">
      <div class="left">
        <div class="logo-wrapper">
          <div class="logo logo-hook" :class="{'highlight':totalCount>0}">
            <span class="icon-shopping_cart" :class="{'highlight':totalCount>0}"></span>
          </div>
          <div class="num" v-show="totalCount>0">{{totalCount}}</div>

        </div>
        <div class="price">￥{{totalPrice}}</div>
        <div class="desc">另需配送费￥{{seller.deliveryPrice}}</div>
      </div>
      <div class="right">
        <div class="pay" :class="{'not-enought':totalPrice<minPrice,'enought':totalPrice>=minPrice}">
          {{payDesc}}
        </div>
      </div>
    </div>
    <div class="ball-container">
      <transition-group name="drop"
                        tag="ul"
                        @before-enter="beforeEnter"
                        @enter="enter"
                        @leave="leave1"
      >
        <li v-for="(ball, index) in balls" :key="index" v-show="ball.show" class="ball">
          <div class="inner inner-hook"></div>
        </li>
      </transition-group>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import Velocity from 'velocity-animate';

  export default {
    props: {
      minPrice: {
        type: Number,
        default: 25
      },
      seller: {
        type: Object,
        default: {}
      },
      selectFoods: {
        type: Array,
        default() {
          return [{
            price: 22,
            count: 1
          }];
        }
      }
    },
    data() {
      return {
        balls: [
          {show: false},
          {show: false},
          {show: false},
          {show: false},
          {show: false}
        ],
        dropBalls: []
      };
    },
    computed: {
      totalPrice() {
        let total = 0;
        this.selectFoods.forEach((food) => {
          total += food.price * food.count;
        });
        return total;
      },
      totalCount() {
        let count = 0;
        this.selectFoods.forEach((food) => {
          count += food.count;
        });
        return count;
      },
      payDesc() {
        if (this.totalPrice === 0) {
          return `￥${this.minPrice}起送`;
        } else if (this.totalPrice < this.minPrice) {
          let cost = this.minPrice - this.totalPrice;
          return `还差￥${cost}起送`;
        } else {
          return '去结算';
        }
      }
    },
    methods: {
      drop(el) {
        for (let i = 0, len = this.balls.length; i < len; i++) {
          let ball = this.balls[i];
          if (!ball.show) {
            ball.show = true;
            ball.el = el;
            this.dropBalls.push(ball);
            return;
          }
        }
      },
      beforeEnter(el) {
        el.style.opacity = 0;
//        let count = this.dropBalls.length;
//        let ball = this.dropBalls[count - 1];
//        if (ball.show) {
//          /*
//          * 定位小球盒子
//          * */
//          let rect = ball.el.getBoundingClientRect();
//          el.style.top = `${rect.top}px`;
//          el.style.left = `${rect.left}px`;
//          /*
//          * 计算小球运动的x，y距离
//          * */
//          let x = -(rect.left - 40);
//          let y = window.innerHeight - rect.top - 28;
//          Velocity(
//            el,
//            {opacity: 1, translateY: `${y}px`},
//            400,
//            [0.49, -0.29, 0.75, 0.41],
//            {complete: done}
//          );
//          let inner = el.getElementsByClassName('inner-hook');
//          Velocity(
//            inner,
//            {opacity: [0, 1], translateX: `${x}px`},
//            400,
//            {complete: done}
//          );
//        }
      },
      enter(el, done) {
        let count = this.dropBalls.length;
        let ball = this.dropBalls[count - 1];
        if (ball.show) {
          /*
          * 定位小球盒子
          * */
          let rect = ball.el.getBoundingClientRect();
          el.style.top = `${rect.top}px`;
          el.style.left = `${rect.left}px`;
          /*
          * 计算小球运动的x，y距离
          * */
          let x = -(rect.left - 40);
          let y = window.innerHeight - rect.top - 28;
          Velocity(
            el,
            {opacity: 1, translateY: `${y}px`},
            400,
            [0.49, -0.29, 0.75, 0.41],
            {complete: done}
          );
          let inner = el.getElementsByClassName('inner-hook');
          Velocity(
            inner,
            {opacity: 1, translateX: `${x}px`},
            400,
            {complete: done}
          );
        }
      },
      leave1: function (el, done) {
        console.log(123);
//        this.dropBalls.shift();
//        console.log(this.balls);
//        for (let i = 0, len = this.balls.length; i < len; i++) {
//          let ball = this.balls[i];
//          if (!ball.show) {
//            ball.show = false;
//            el = '';
//            return;
//          }
//        }
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .shopcart
    position: fixed
    left: 0
    bottom: 0
    width: 100%
    height: 48px
    z-index: 50
    .content
      display: flex
      background: #141d27
      .left
        flex: 1
        .logo-wrapper
          display: inline-block
          position: relative
          top: -10px
          margin: 0 12px
          width: 56px
          height: 56px
          line-height: 56px
          border-radius: 50%
          background: #141d27
          text-align: center
          font-size: 0
          .logo
            display: inline-block
            margin-top: 6px;
            width: 44px
            height: 44px
            border-radius: 50%
            background: #2b343c
            &.highlight
              background: rgb(0, 160, 220)
            .icon-shopping_cart
              font-size: 24px
              line-height: 44px
              color: #80858a
              &.highlight
                color: #fff
          .num
            position: absolute
            top: 0
            right: 0
            padding: 0 4px
            height: 16px
            line-height: 16px
            text-align: center
            border-radius: 16px
            font-size: 9px
            font-weight: 700
            color: #fff
            background: rgb(240, 20, 20)
            box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.4)
        .price
          vertical-align: top
          display: inline-block
          margin-top: 12px
          margin-left: 15px
          padding-right: 12px
          font-size: 16px
          font-weight: 700
          line-height: 24px
          color: rgba(255, 255, 255, 0.4)
          border-right: 1px solid rgba(255, 255, 255, 0.1)
        .desc
          display: inline-block
          vertical-align: top
          padding-left: 12px
          font-size: 12px
          line-height: 48px
          font-weight: 200
          color: rgba(255, 255, 255, 0.4)

      .right
        flex: 0 0 105px
        background: #2b343c
        font-size: 14px
        font-weight: 700
        line-height: 48px
        color: rgba(255, 255, 255, 0.4)
        text-align: center
        .pay
          &.not-enought
            background: #2b333b
          &.enought
            background: #00b43c
            color: #fff
    .ball-container
      span
        .ball
          position: fixed
          z-index: 200
          .inner
            width: 16px
            height: 16px
            border-radius: 50%
            background: rgb(0, 160, 220)
</style>
