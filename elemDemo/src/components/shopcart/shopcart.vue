<template>
 <div class="shopcart">
     <div class="content" @click="toggleList">
         <div class="content-left">
             <div class="logo-wrapper">
                 <div class="logo" :class="{'highlight':totalcount > 0}">
                     <i  :class="{'highlight':totalcount > 0}" class="icon-shopping_cart"></i>
                 </div>
                 <div  v-if="totalcount > 0" class="num" >{{totalcount}}</div>
             </div>
             <div :class="{'highlight':totalcount > 0}" class="price">￥{{totalprice}}</div>
             <div class="desc">另需配送费￥{{deliveryprice}}</div>
         </div>
         <div class="content-right"  @click.stop.prevent="pay">
             <div class="pay" :class="payclass">
                 {{paydesc}}
             </div>
         </div>
     </div> 
     <div class="ball-container">    
       <div v-for="ball in balls">
         <transition v-on:before-enter = "beforeEnter" v-on:enter = "enter" v-on:after-enter = "afterEnter" name="drop">
         <div  class="ball"  v-show="ball.show">
             <div class="inner inner-hook">
               </div> 
         </div>
         </transition>      
       </div>
          
     </div>
     <transition name="fold">
     <div class="shopcart-list"  v-if="listshow">
       <div class="list-header">
         <h1 class="title">购物车</h1>
         <span class="empty" @click="clearFoods">
           清空
         </span>
       </div>
       <div class="list-content" ref="listWrapper">
         <ul>
           <li class="food" v-for="item in selectFoods">
             <span class="name">{{item.name}}</span>
             <div class="price">
               <span>￥{{item.price * item.count}}</span>
             </div>
             <div class="cartcontrol-wrapper">
                <v-cartconcontrol :foo="item" @add="addFood"></v-cartconcontrol>
             </div>
           </li>
         </ul>
       </div>
     </div>
     </transition>
      <transition name="mask">
 <div class="list-mask" @click="closeAll" v-show="listshow"></div>
 </transition>
 </div>

</template>

<script>
import BScroll from 'better-scroll'
import VCartconcontrol from 'components/cartconcontrol/cartconcontrol'
export default {
  components: {
    VCartconcontrol
  },
  props: {
    selectFoods: {
      type: Array
    },
    deliveryprice: {
      type: Number
    },
    minprice: {
      type: Number
    }
  },
  data () {
    return {
      balls: [{
        show: false
      }, {
        show: false
      }, {
        show: false
      }, {
        show: false
      }, {
        show: false
      }],
      dropballs: [],
      fold: true
    }
  },
  computed: {
    totalprice () {
      let total = 0
      this.selectFoods.forEach(food => {
        total += food.price * food.count
      })
      return total
    },
    totalcount () {
      let total = 0
      this.selectFoods.forEach(food => {
        total += food.count
      })
      return total
    },
    paydesc () {
      if (this.totalprice === 0) {
        return `￥ ${this.minprice}元起送`
      } else if (this.totalprice < this.minprice) {
        let diff = this.minprice - this.totalprice
        return '还差￥' + diff + '元起送'
      } else {
        return `去结算`
      }
    },
    payclass () {
      if (this.totalprice < this.minprice) {
        return 'not-enough'
      } else {
        return 'enough'
      }
    },
    listshow () {
      if (this.totalcount === 0) {
        this.fold = true
        return false
      }
      let show = !this.fold
      if (show) {
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.listWrapper, {click: true})
          } else {
            this.scroll.refresh()
          }
        })
      }
      return show
    }
  },
  methods: {
    drop (el) {
      for (let i = 0; i < this.balls.length; i++) {
        let ball = this.balls[i]
        if (!ball.show) {
          ball.show = true
          ball.el = el
          this.dropballs.push(ball)
          return
        }
      }
    },
    addFood (target) {
      this.drop(target)
    },
    pay () {
      if (this.totalprice < this.minprice) {
        return false
      } else {
        alert(`pay ${this.totalprice}`)
      }
    },
    beforeEnter: function (el) {
      let count = this.balls.length
      while (count--) {
        let ball = this.balls[count]
        if (ball.show) {
          let rect = ball.el.getBoundingClientRect()
          let x = rect.left - 32
          let y = -(window.innerHeight - rect.top - 22)
          el.style.display = ''
          el.style.webkitTransform = `translate3d(0,${y}px,0)`
          el.style.transform = `translate3d(0,${y}px,0)`
          let inner = el.getElementsByClassName('inner-hook')[0]
          inner.style.webkitTransform = `translate3d(${x}px,0,0)`
          inner.style.transform = `translate3d(${x}px,0,0)`
        }
      }
    },
    enter: function (el, done) {
      /* eslint-disable no-unused-vars */
      let rf = el.offsetHeight
      this.$nextTick(() => {
        el.style.webkitTransform = 'translate3d(0,0,0)'
        el.style.transform = 'translate3d(0,0,0)'
        let inner = el.getElementsByClassName('inner-hook')[0]
        inner.style.webkitTransform = 'translate3d(0,0,0)'
        inner.style.transform = 'translate3d(0,0,0)'
        el.addEventListener('transitionend', done)
      })
    },
    afterEnter: function (el) {
      let ball = this.dropballs.shift()
      if (ball) {
        ball.show = false
        el.style.display = 'none'
      }
    },
    toggleList () {
      if (!this.totalcount) {
        return
      }
      this.fold = !this.fold
    },
    closeAll () {
      this.fold = true
    },
    clearFoods () {
      this.selectFoods.forEach((food) => {
        food.count = 0
      })
    }
  }
}
</script>
<style scoped rel="stylesheet/stylus" lang="stylus">
@import '../../common/stylus/mixin.styl'
.shopcart 
  position: fixed
  left: 0
  bottom: 0
  z-index: 50
  width: 100%
  height: 48px

  .content {
    display: flex
    background-color: #141d27
    font-size: 0

    .content-left {
      flex: 1

      .logo-wrapper {
        display: inline-block
        position: relative
        top: -10px
        margin: 0 12px 0 12px
        padding: 5px
        width: 56px
        height: 56px
        box-sizing: border-box
        border-radius: 50%
        vertical-align: top
        background-color: #141d27

        & .logo {
          width: 100%
          height: 100%
          border-radius: 50%
          background-color: #2b343c
          text-align: center

          &.highlight {
            background-color: rgb(0, 160, 220)
          }

          .icon-shopping_cart {
            font-size: 24px
            color: #80858a
            line-height: 44px

            &.highlight {
              color: #fff
            }
          }
        }

        .num {
          position: absolute
          top: 0
          right: 0
          width: 24px
          height: 16px
          line-height: 16px
          text-align: center
          border-radius: 16px
          font-size: 9px
          font-weight: 700
          color: #ffffff
          background-color: rgb(240, 20, 20)
          box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.4)
        }
      }

      .price {
        display: inline-block
        vertical-align: top
        line-height: 24px
        margin-top: 12px
        box-sizing: border-box
        padding-right: 12px
        border-right: 1px solid rgba(255, 255, 255, 0.1)
        font-size: 16px
        font-weight: 700
        color: rgba(255, 255, 255, 0.4)

        &.highlight {
          color: #fff
        }
      }

      .desc {
        display: inline-block
        vertical-align: top
        margin: 12px 0 0 12px
        line-height: 24px
        font-size: 13px
        color: rgba(255, 255, 255, 0.4)
      }
    }

    .content-right {
      flex: 0 0 105px
      width: 105px

      .pay {
        line-height: 48px
        text-align: center
        font-size: 12px
        color: rgba(255, 255, 255, 0.4)
        font-weight: 700
        background-color: #2b333b
        &.not-enough {
          background: #2b333b
        }
        &.enough {
          background: #00b43c
          color: #fff
        }
      }
    }
  }
  .ball-container
    .ball
      position fixed
      left 32px
      bottom 22px
      z-index 100
      &.drop-enter-active 
        transition: all 0.5s cubic-bezier(0.49,-0.29,0.75,0.41)
      &.drop-leave-active 
        transition: all 0.5s cubic-bezier(0.49,-0.29,0.75,0.41)
      .inner
        width 16px
        height 16px
        border-radius 50%
        background-color rgb(0, 160, 220)
        transition: all 0.5s linear
  .shopcart-list
    position: absolute
    left: 0
    top: 0
    z-index: -1
    width: 100%
    transform: translate3d(0, -100%, 0)
    &.fold-enter-active, &.fold-leave-active
      transition: all 0.5s
    &.fold-enter, &.fold-leave-active
      transform: translate3d(0, 0, 0)
    .list-header
      height: 40px
      line-height: 40px
      padding: 0 18px
      background: #f3f5f7
      border-bottom: 1px solid rgba(7, 17, 27, 0.1)
      .title
        float: left
        font-size: 14px
        color: rgb(7, 17, 27)
      .empty
        float: right
        font-size: 12px
        color: rgb(0, 160, 220)
    .list-content
      padding: 0 18px
      max-height: 217px
      overflow: hidden
      background: #fff
      .food
        position: relative
        padding: 12px 0
        box-sizing: border-box
        border-1px(rgba(7,17,27,0.1))
        .name
          line-height: 24px
          font-size: 14px
          color: rgb(7, 17, 27)
        .price
          position: absolute
          right: 90px
          bottom: 12px
          line-height: 24px
          font-size: 14px
          font-weight: 700
          color: rgb(240, 20, 20)
        .cartcontrol-wrapper
          position: absolute
          right: 0
          bottom: 6px
.list-mask
  position fixed
  top 0
  left 0
  width 100%
  height 100%
  background rgba(7,17,27,0.5)
  backdrop-fillter blur(10px)
  z-index -2
  &.mask-enter-active 
    transition all .5s ease
  &.mask-leave-active
    transition all .5s ease
  &.mask-enter
    opacity 0 
  &.mask-leave-to
    opacity 0
</style>
