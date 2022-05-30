<!--  -->
<template>
  <div class='goods' >
  <div class="menu-wrapper" ref="menuWrapper">
  <ul  class="menu-title">
  <li v-for=" (good, index) in goods" :class="{'current':currentIndex === index}"  @click = "selectMenu(index, $event)" >
   <span class="text border-1px"><v-icon  v-show="good.type > 0" :stype="good.type"></v-icon>{{good.name}}</span>
  </li>
  </ul>
  </div>
  <div class="foods-wrapper" ref="foodWrapper">
    <ul>
      <li class="food-list food-list-hook" v-for="good in goods">
        <h1 class="title">{{good.name}}</h1>
        <ul>
          <li class="food-item border-1px" @click="selectFood(food, $event)" v-for="food in good.foods">
            <div class="icon">
              <img width="57" :src="food.icon"/>
            </div>
            <div class="content">
              <h2 class="name">{{food.name}}</h2>
              <p class="desc">{{food.description}}</p>
              <div class="extra">
                <span class="count">月售{{food.sellCount}}</span>
                <span>好评率{{food.rating}}%</span>
              </div>
              <div class="price">
                <span class="now">${{food.price}}</span>
                <span class="old" v-if="food.oldPrice">${{food.oldPrice}}</span>
              </div>
              <div class="cartcontrol-wrapper">
                  测试
                 <v-cartconcontrol :foo="food" @add="cartdrop($event)"></v-cartconcontrol>
              </div>
            </div>
          </li>
        </ul>
      </li>
    </ul>
  </div>
  <v-shopcart :selectFoods="selectFoods" :deliveryprice="this.seller.deliveryPrice" :minprice="this.seller.minPrice" ref="VShopcart"></v-shopcart>
  
  <v-food :food="selectedFood" ref="food" @add="cartdrop($event)"></v-food>
  </div>
</template>

<script>
import VIcon from 'components/icon/icon'
import BScroll from 'better-scroll'
import VShopcart from 'components/shopcart/shopcart'
import VFood from 'components/food/food'
import VCartconcontrol from 'components/cartconcontrol/cartconcontrol'
const ERR_OK = 0
export default {
  components: {
    VIcon, VShopcart, VCartconcontrol, VFood
  },
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      goods: [],
      listHeight: [],
      scrollY: 0,
      selectedFood: {}
    }
  },
  methods: {
    _initScroll () {
      this.menuScroll = new BScroll(this.$refs.menuWrapper, {click: true})
      this.foodScroll = new BScroll(this.$refs.foodWrapper, {probeType: 3, click: true})
      this.foodScroll.on('scroll', (pos) => {
        this.scrollY = Math.abs(Math.round(pos.y))
      })
    },
    _calculateHeight () {
      let foodList = this.$refs.foodWrapper.getElementsByClassName('food-list-hook')
      let height = 0
      this.listHeight.push(height)
      for (let i = 0; i < foodList.length; i++) {
        height += foodList[i].clientHeight
        this.listHeight.push(height)
      }
    },
    selectMenu (index, event) {
      if (!event._constructed) {
        return
      }
      // let foodList = this.$refs.foodWrapper.getElementsByClassName('food-list-hook')
      // let el = foodList[index]
      // this.foodScroll.scrollToElement(el, 300)
      this.foodScroll.scrollTo(0, -(this.listHeight[index]), 300)
    },
    cartdrop (target) {
      this.$nextTick(() => {
        this.$refs.VShopcart.addFood(target)
      })
    },
    selectFood (food, event) {
      if (!event._constructed) {
        return
      }
      this.selectedFood = food
      this.$refs.food.show()
    }
  },
  created () {
    this.$http.get('/api/goods').then((data) => {
      data = data.body
      if (data.errno === ERR_OK) {
        this.goods = data.data
        this.$nextTick(() => {
          this._initScroll()
          this._calculateHeight()
        })
      }
    })
  },
  computed: {
    currentIndex () {
      for (let i = 0; i < this.listHeight.length; i++) {
        let height1 = this.listHeight[i]
        let height2 = this.listHeight[i + 1]
        if (!height2 || (this.scrollY >= height1 && this.scrollY < height2 - 1)) {
          return i
        }
      }
      return 0
    },
    selectFoods () {
      let foods = []
      this.goods.forEach((good) => {
        good.foods.forEach((food) => {
          if (food.count > 0) {
            foods.push(food)
          }
        })
      })
      return foods
    }
  }
}
</script>
<style lang='stylus' rel='stylesheet/stylus' scoped>
@import '../../common/stylus/mixin.styl'
.goods
  display flex
  position absolute
  top 174px
  bottom 46px
  width 100%
  overflow hidden
  .menu-wrapper
    flex 0 0 80px
    width 80px
    background #f3f5f7
    .menu-title
      & li
        display table
        padding 0 12px
        width 56px
        height 54px
        line-height 14px
        font-size 0
        &.current
          position relative
          margin-top -1px
          z-index 10
          background-color #ffffff
          font-weight 700
          .text
            border-none()
        & >.text
          display table-cell    
          font-size 12px
          border-1px(rgba(7,17,27,0.1))
          vertical-align middle
          .icon
            display inline-block
            width 13px
            height 13px
            margin-right 1px
            background-size 13px 13px
            vertical-align bottom  
  .foods-wrapper
    flex 1 
    .food-list
      .title
        width 100%
        height 26px
        line-height 26px
        font-size 12px
        color rgb(147,153,159)
        padding-left 14px
        border-left 2px solid #d9dde1
        background-color  #f3f5f7
      .food-item
        display flex
        margin 18px 18px 0 18px
        padding-bottom 18px
        border-1px(rgba(7,17,27,0.1))
        &:last-child
          border-none()
        .icon
          flex 0 0 57px
          margin-right 10px
        .content
          flex 1
          .name
            font-size 14px
            margin 2px 0 8px 0
            height  14px
            line-height 14px
            color rgb(7,17,27)
          .desc,.extra
            line-height 12px
            color rgb(147,153,159)
            font-size 10px
          .desc 
            margin-bottom 8px 
            .extra
             & .count
               margin-right 12px 
          .price
            font-weight 700
            line-height 24px
            .now
              margin-right 18px
              font-size 14px
              color rgb(240,20,20)
            .old
              text-decoration line-through
              font-size 10
              color rgb(147,153,159)
          .cartcontrol-wrapper
            position absolute
            right 0
            bottom 12px

            

</style>