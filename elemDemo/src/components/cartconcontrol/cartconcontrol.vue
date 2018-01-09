<template>
 <div class="cartcontrol">
   <transition name="fade">
     <div class="cart-decrease icon-remove_circle_outline" @click.stop.prevent="decreaseCart" v-show="foo.count > 0"></div>
   </transition>
   <div class="cart-count" v-show="foo.count > 0">{{foo.count}}</div>
   <div class="cart-add icon-add_circle" @click.stop.prevent="addCart"></div>
 </div>
</template>

<script type="text/ecmascript-6">
import Vue from 'vue'
export default {
  props: {
    foo: {
      type: Object
    }
  },
  data () {
    return {}
  },
  methods: {
    addCart (event) {
      if (!this.foo.count) {
        Vue.set(this.foo, 'count', 1)
      } else {
        console.log(this.foo)
        this.foo.count += 1
      }
      this.$emit('add', event.target)
    },
    decreaseCart (event) {
      if (this.foo.count) {
        this.foo.count -= 1
      }
    }
  },
  components: {}
}
</script>

<style scoped rel="stylesheet/stylus" lang="stylus">
.cartcontrol {
  font-size: 0

  & > div {
    display: inline-block
    line-height: 24px
    vertical-align: top
  }

  .cart-decrease {
    padding: 6px
    font-size: 24px
    color: rgb(0, 160, 220)

    &.fade-enter-active {
      transition: all 0.4s linear
    }

    &.fade-leave-active {
      transition: all 0.4s linear
    }

    &.fade-enter {
      opacity: 0
      transform: translate3D(0, 0, 0)
    }

    &.fade-leave-to {
      opacity: 0
      transform: translate3D(25px, 0, 0)
    }
  }

  .cart-count {
    width: 12px
    font-size: 12px
    padding-top: 6px
    color: rgb(147, 153, 159)
  }

  .cart-add {
    padding: 6px
    font-size: 24px
    color: rgb(0, 160, 220)
  }
}
</style>
