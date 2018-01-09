<template>
  <div>
  <div class="header">
    <v-header :seller="seller"></v-header>
  </div>
  <div class="tab border-1px">
  <div class="tab-item"><router-link tag="a" :to="{path: '/goods'}">商品</router-link> </div>
  <div class="tab-item" ><router-link tag="a" :to="{path: '/ratings'}">评论</router-link> </div>
  <div class="tab-item"><router-link tag="a" :to="{path: '/seller'}">商家</router-link></div>
  </div>
 
   <keep-alive><router-view :seller="seller"></router-view></keep-alive>
  </div>
</template>

<script>
import VHeader from './components/header/header'
import {urlParse} from 'common/js/util'
const ERR_OK = 0

export default {
  name: 'app',
  components: {
    VHeader
  },
  created () {
    this.$http.get('/api/seller?id=' + this.seller.id + '').then((res) => {
      res = res.body
      if (res.errno === ERR_OK) {
        this.seller = Object.assign({}, this.seller, res.data)
        console.log(this.seller)
      }
    })
  },
  data () {
    return {
      seller: {
        id: (() => {
          let queryParam = urlParse()
          return queryParam.id
        })()
      }
    }
  }
}
</script>

<style lang='stylus' rel='stylesheet/stylus'>
@import './common/stylus/mixin.styl'

.tab {
  display: flex
  width: 100%
  height: 40px
  line-height: 40px
  justify-content: center
  // border-bottom 1px solid rgba(7,17,27,0.1)
  border-1px(rgba(7, 17, 27, 0.1))

  & > div {
    flex: 1
    text-align: center

    & > a {
      display: block
      font-size: 14px
      color: rgb(77, 85, 93)

      &.router-link-active {
        color: rgb(240, 20, 20)
      }
    }
  }
}
</style>
