<template>
  <div>
    <div class="goods">
      <cube-scroll-nav
        :slide=true
        :data="goods"
        :options="scrollOptions"
        v-if="goods.length">
        <cube-scroll-nav-panel
          v-for="good in goods"
          :key="good.name"
          :label="good.name"
          :title="good.name">
          <ul>
            <li v-for="food in good.foods" :key="food.name" class="food-item">
              <div class="icon"><img width="57" height="57" :src="food.icon"></div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="count">月售{{food.sellCount}}份</span>
                  <span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">￥{{food.price}}</span>
                  <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                </div>
              </div>
            </li>
          </ul>
        </cube-scroll-nav-panel>
      </cube-scroll-nav>
      <div class="shop-cart-wrapper">
        <shopcart
          :delivery-price="seller.deliveryPrice"
                  :min-price="seller.minPrice"></shopcart>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll';
  import shopcart from 'components/shopcart/shopcart';
  import food from 'components/food/food';
  import { getGoods } from 'api';

  const ERR_OK = 0;
  const debug = process.env.NODE_ENV !== 'production';

  export default {
    name: 'goods',
    props: {
      data: {
        type: Object,
        default() {
          return {}
        }
      }
    },
    data() {
      return {
        goods: [],
        scrollOptions: {
          click: false,
          directionLockThreshold: 0
        }
      };
    },
    computed: {
      seller() {
        return this.data.seller
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
    methods: {
      fetch() {
        getGoods().then((goods) => {
          this.goods = goods
        })
      }
    },
    components: {
      shopcart,
      food
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"
  @import "../../common/stylus/variable.styl"

  .goods
    position: relative
    text-align: left
    height: 100%

    .scroll-nav-wrapper
      position: absolute
      width: 100%
      top: 0
      left: 0
      bottom: 48px

    >>> .cube-scroll-nav-bar
      width: 80px
      white-space: normal
      overflow: hidden

    >>> .cube-scroll-nav-bar-item
      padding: 0 10px
      display: flex
      align-items: center
      height: 56px
      line-height: 14px
      font-size: $fontsize-small
      background: $color-background-ssss

      .text
        flex: 1
        position: relative

      .num
        position: absolute
        right: -8px
        top: -10px

      .support-ico
        display: inline-block
        vertical-align: top
        margin-right: 4px

    >>> .cube-scroll-nav-bar-item_active
      background: $color-white
      color: $color-dark-grey

    >>> .cube-scroll-nav-panel-title
      padding-left: 14px
      height: 26px
      line-height: 26px
      border-left: 2px solid $color-col-line
      font-size: $font-size-small
      color: $color-grey
      background: $color-background-ssss

    .food-item
      display: flex
      margin: 18px
      padding-bottom: 18px
      position: relative

      &:last-child
        border-none()
        margin-bottom: 0

      .icon
        flex: 0 0 57px
        margin-right: 10px

        img
          height: auto

      .content
        flex: 1

        .name
          margin: 2px 0 8px 0
          height: 14px
          line-height: 14px
          font-size: $fontsize-medium
          color: $color-dark-grey

        .desc, .extra
          line-height: 10px
          font-size: $fontsize-small-s
          color: $color-light-grey

        .desc
          line-height: 12px
          margin-bottom: 8px

        .extra
          .count
            margin-right: 12px

        .price
          font-weight: 700
          line-height: 24px

          .now
            margin-right: 8px
            font-size: $fontsize-medium
            color: $color-red

          .old
            text-decoration: line-through
            font-size: $fontsize-small-s
            color: $color-light-grey

      .cart-control-wrapper
        position: absolute
        right: 0
        bottom: 12px

    .shop-cart-wrapper
      position: absolute
      left: 0
      bottom: 0
      z-index: 50
      width: 100%
      height: 48px
</style>
