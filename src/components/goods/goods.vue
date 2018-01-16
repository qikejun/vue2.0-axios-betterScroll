<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menu">
      <ul>
        <li v-for="item,index in goods" class="menu-item" :class="{'active':index === menuIndex}"
            @click="checkedMenu(index)">
          <span class="text border-1px">
            <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foods">
      <ul>
        <li v-for="item in goods" class="food-list">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" class="food-item">
              <div class="icon">
                <img :src="food.icon" alt="" width="57" height="57">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="left">月售{{food.sellCount}}</span><span>好评率{{food.rating}}</span>
                </div>
                <div class="price">
                  <span class="rmb">￥</span><span class="now">{{food.price}}</span><span v-show="food.oldPrice"
                                                                                         class="old">￥{{food.oldPrice}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <cartcontrol :food="food" @cartadd="_drop"></cartcontrol>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shop-cart ref="shopcart" :select-foods="selectFoods" :seller="seller"></shop-cart>
  </div>
</template>

<script type="text/ecmascript-6">
  import axios from 'axios';
  import BScroll from 'better-scroll';
  import shopCart from './../shopCart/shopCart';
  import cartcontrol from './../cartcontrol/cartcontrol';

  const ERR_OK = 0;
  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        goods: [],
        heightMap: [],
        scrollY: 0
      };
    },
    computed: {
      menuIndex() {
        for (let i = 0, length = this.heightMap.length; i < length; i++) {
          let height1 = this.heightMap[i];
          let height2 = this.heightMap[i + 1];
          if (!height2 || this.scrollY >= height1 && this.scrollY < height2) {
            return i;
          }
        }
      },
      selectFoods() {
        let foods = [];
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count) {
              foods.push(food);
            }
          });
        });
        return foods;
      }
    },
    created() {
      axios({
        method: 'get',
        url: '/api/goods'
      })
        .then(response => {
          if (response.data.erron === ERR_OK) {
            this.goods = response.data.data;
            this.$nextTick(() => {
              this._initScroll();
              this._calculateHeight();
            });
          }
        })
        .catch(error => {
          console.log(error);
          alert('网络错误，不能访问');
        });
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    methods: {
      _initScroll() {
        this.menuScroll = new BScroll(this.$refs.menu, {scrollY: true, click: true});
        this.foodsScroll = new BScroll(this.$refs.foods, {scrollY: true, click: true, probeType: 3});
        this.foodsScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.floor(pos.y));
          //  获取foods-wrapper面板的高度
          //  修复最后一列商品少无法选中问题
          let foodsWrapperHeight = document.querySelector('.foods-wrapper').clientHeight;
          if (this.scrollY + foodsWrapperHeight >= this.heightMap[this.heightMap.length - 1] - 10) {
            this.scrollY = this.heightMap[this.heightMap.length - 1] - 10;
          }
        });
      },
      _calculateHeight() {
        this.heightMap = [];
        let foodList = document.querySelectorAll('.food-list');
        let height = 0;
        this.heightMap.push(height);
        for (let i = 0, length = foodList.length; i < length; i++) {
          height += foodList[i].clientHeight;
          this.heightMap.push(height);
        }
      },
      _drop(target) {
        this.$refs.shopcart.drop(target);
      },
      checkedMenu(index) {
        let foodList = document.querySelectorAll('.food-list');
        this.foodsScroll.scrollToElement(foodList[index], 200);
      }
    },
    components: {
      shopCart, cartcontrol
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl";
  .goods
    display: flex
    position: absolute
    top: 174px
    bottom: 46px
    width: 100%
    overflow: hidden
    .menu-wrapper
      flex: 0 0 80px
      background: #f5f3f7
      .menu-item
        display: table
        width: 100%
        height: 54px
        line-height: 14px
        font-weight: 200
        border-1px (rgba(7, 17, 27, 0.1))
        &.active
          position: relative
          margin-top: -1px
          padding-top: 1px
          font-weight: 700
          border-1px (#ff)
          background: #fff
          z-index: 10
          .text
            border-none()
        .icon
          vertical-align: top
          display: inline-block
          width: 12px
          height: 12px
          margin-right: 2px
          background-size: 12px 12px
          background-repeat: no-repeat
          &.decrease
            bg-image('decrease_3')
          &.discount
            bg-image('discount_3')
          &.guarantee
            bg-image('guarantee_3')
          &.invoice
            bg-image('invoice_3')
          &.special
            bg-image('special_3')
        .text
          display: table-cell
          padding: 0 12px
          width: 56px
          vertical-align: middle
          font-size: 12px
          border-1px (rgba(7, 17, 27, 0.1))
    .foods-wrapper
      flex: 1
      background: #fff
      .title
        padding-left: 14px
        height: 26px
        line-height: 26px
        font-size: 12px
        color: rgb(147, 153, 159)
        border-left: 2px solid #d9dde1
        background: #f3f5f7
      .food-item
        display: flex
        margin: 18px 18px 0;
        padding-bottom: 36px;
        border-1px (rgba(7, 17, 27, 0.1))
        &:last-child
          border-none()
          margin-bottom: 0
        .icon
          flex: 0 0 57px
          margin-right: 10px
          img
            border-radius: 2px
        .content
          flex: 1
          .name
            padding: 2px 0 8px
            font-size: 14px
            line-height: 14px
            font-weight: 700
            color: rgb(7, 17, 27)
          .desc
            line-height: 14px
            font-size: 12px
            color: rgb(147, 153, 159)
            padding-bottom: 8px
          .extra
            font-size: 12px
            color: rgb(147, 153, 159)
            padding-bottom: 8px
            .left
              margin-right: 12px
          .price
            line-height: 24px
            font-weight: 700
            .now
              font-size: 14px
              color: rgb(240, 20, 20)
              margin-right: 8px
              .rmb
                font-size: 10px
            .old
              font-size: 10px
              color: rgb(147, 153, 159)
              text-decoration: line-through
          .cartcontrol-wrapper
            position: absolute
            right: 0
</style>
