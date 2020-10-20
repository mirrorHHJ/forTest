<template>
  <div>
    <v-header :seller="seller"></v-header>
    <div class="tab-wrapper">
      <v-tab :tabs="tabs" :initialIndex="0"></v-tab>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import header from 'components/header/header.vue'
  import Goods from 'components/goods/goods'
  import Ratings from 'components/ratings/ratings'
  import Seller from 'components/seller/seller'
  import tab from 'components/tab/tab.vue'
  import { getSeller } from 'api'

  export default {
    data() {
      return {
        seller: {}
      }
    },
    computed: {
      tabs() {
        return [
          {
            label: '商品',
            component: Goods,
            data: {
              seller: this.seller
            }
          }, {
            label: '评价',
            component: Ratings,
            data: {
              seller: this.seller
            }
          }, {
            label: '商家',
            component: Seller,
            data: {
              seller: this.seller
            }
          }
        ]
      }
    },
    created() {
      this._getSeller()
    },
    methods: {
      _getSeller() {
        getSeller().then((seller) => {
          this.seller = seller
        })
      }
    },
    components: {
      'v-header': header,
      'v-tab': tab
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "./common/stylus/mixin.styl"

  .tab-wrapper
    position: fixed
    top: 136px
    left: 0
    bottom: 0
    right: 0

  .tab
    display: flex
    width: 100%
    height: 40px
    line-height: 40px
    border-1px(rgba(7, 17, 27, 0.1))

    .tab-item
      flex: 1
      text-align: center

      & > a
        display: block
        font-size: 14px
        color: rgb(77, 85, 93)

        &.active
          color: rgb(240, 20, 20)
</style>
