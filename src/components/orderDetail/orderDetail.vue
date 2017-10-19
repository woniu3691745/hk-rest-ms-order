<template lang="html">
  <transition name="move">
    <div class="detailWrapper" ref="detailWrapper" v-show="showDetail">
      <div class="orderDetail">
        <div class="list-header">
            <div class="back" @click="showToggle()">
              <i class="icon-arrow_lift"></i>
            </div>
        </div>
         <div class="list-content" ref="foodlist">
            <ul>
              <li class="food" v-for="foodTemp in food">
                <span class="name">{{foodTemp.dishesName}}</span>
                <div class="price">
                  <span>￥{{foodTemp.dishesPriceNow * foodTemp.count}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <cartcontrol :food="foodTemp"></cartcontrol>
                </div>
              </li>
            </ul>
          </div>
      </div>
    </div>
  </transition>
</template>

<script>
import '../../filter/time.js'
import BScroll from 'better-scroll'
import cartcontrol from 'components/cartcontrol/cartcontrol'

export default {
  components: {
    cartcontrol
  },
  props: {
    food: Array
  },
  data() {
    return {
      showDetail: false,
      evelflag: true
    }
  },
  computed: {
    evelArr() {
      let selectIndex = 0
      if (this.detailWrapper) {
        this.$nextTick(() => {
          this.detailWrapper.refresh()
        })
      }
      return selectIndex ? this.food.ratings.filter((data) => this.evelflag ? data.rateType === selectIndex - 1 && data.text : data.rateType === selectIndex - 1) : this.food.ratings.filter((data) => this.evelflag ? data.text : true)
    }
  },
  methods: {
    showToggle() {
      debugger;
      this.showDetail = !this.showDetail
      if (this.showDetail) {
        this.$nextTick(() => {
          this._initScroll()
        })
      }
    },
    _initScroll() {
      if (!this.detailWrapper) {
        this.detailWrapper = new BScroll(this.$refs.detailWrapper, {
          click: true
        });
      } else {
        this.detailWrapper.refresh()
      }
    },
    addCart(event) {
      if (!event._constructed) {
        return
      }
      this.$set(this.food, 'count', 1)
      this.$root.eventHub.$emit('cart.add', event.target)
    },
    filterEvel(item) {
      item.active = true
    }
  }
}

</script>

<style lang="stylus" scoped>
.detailWrapper
  position fixed
  z-index 3
  left 0
  top 0
  bottom 48px
  width 100%
  background white
  transition all 0.4s ease
  &.move-enter-avtive,&.move-leave-active{
    transform translate3d(0,0,0)
  }
  &.move-enter,&.move-leave-active{
    transform translate3d(100%,0,0)
  }
.orderDetail
  .list-header
      height 40px
      line-height 40px
      background #f3f5f7
      border-bottom 1px solid rgba(7,17,27,0.1)
      .title
        display inline-block
        margin-left 20px
        font-size 14px
        font-weight 400
        color rgb(7,17,27)
        padding-left 18px
      .empty
        position absolute
        right 8px
        font-size 12px
        color rgb(0,160,220)
        padding 0 10px
      .back
        position absolute
        font-weight 800
        color #333
        left 6px
        font-size 14px
  .list-content
      max-height 217px
      overflow hidden
      .food
        position relative
        display flex
        height 48px
        margin 0 18px
        border-bottom 1px solid rgba(7,17,27,0.1)
        .name
          flex 1
          font-size 14px
          color rgb(7,17,27)
          line-height 48px
          font-weight 700
        .price
          font-size 14px
          font-weight 700
          color rgb(240,20,20)
          padding 0 12px 0 18px
          line-height 48px
        .cartcontrol-wrapper
          font-size 14px
          margin-top 6px
</style>
