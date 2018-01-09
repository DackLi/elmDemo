
<template>
  <div class='header'>
    <span></span>
   <div class="content-wrapper">
     <div class="avatar">
       <img width="64" height="64" :src="seller.avatar" alt="">
     </div>
     <div class="content">
       <div class="title">
         <span></span><p v-text="seller.name"></p>
       </div>
       <div class="delivery">
         {{seller.description + "/" + seller.deliveryTime + "分钟送达"}}
       </div>
       <div v-if="seller.supports" class="activity">
         <v-icon :stype="seller.supports[0].type"></v-icon>
         <p>{{seller.supports[0].description}}</p>
       </div>
       <div v-if="seller.supports" class="activity-num">
          <span  class="num">{{seller.supports.length+"个"}}</span>
          <span @click="showData" class="icon-arrow_lift font-icon"></span>
       </div>
     </div>
   </div>
   <div class="bulletin-wrapper">
     <span class="icon"></span><p>{{seller.bulletin}}</p>
     <span @click="showData" class="icon-arrow_lift font-icon"></span>
   </div>
   <div class="backgrund">
     <img :src="seller.avatar" width="100%" height="100%" alt="">
   </div>
    <transition name="fade">
   <div v-show="detailshow" class="detail" >
     <div class="detail-wrapper closefix">
      <div class="detall-main">
       <h1 class="name">{{seller.name}}</h1>
       <div class="vstar">
          <v-star :size="48" :score="seller.score"></v-star>
       </div> 
       <div class="title">
         <div class="line"></div>
         <div class="text">优惠信息</div>
         <div class="line"></div>       
       </div>    
        <ul class="supportss" v-if="seller.supports">
           <li v-for="item in seller.supports">
             <v-icon :stype="item.type"></v-icon>
              <p>{{item.description}}</p>
           </li>
         </ul>
       <div class="title">
         <div class="line"></div>
         <div class="text">商家公告</div>
         <div class="line"></div>       
       </div> 
       <div class="bull">
         <p>{{seller.bulletin}}</p>
       </div>
      </div>
     </div>
     <div @click="closeData" class="detail-close">
       <i class="icon-close"></i>
     </div>
   </div>
   </transition>
  </div>
</template>

<script type='text/ecmascript-6'>
import VStar from 'components/star/star'
import VIcon from 'components/icon/icon'

export default {
  components: {
    VStar, VIcon
  },
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      detailshow: false
    }
  },
  methods: {
    showData () {
      this.detailshow = true
    },
    closeData () {
      this.detailshow = false
    }
  },
  created () {
    this.classmap = ['decrease', 'discount', 'guarantee', 'invoice', 'special']
  }
}
</script>
<style lang='stylus' rel='stylesheet/stylus' scoped>
@import '../../common/stylus/mixin.styl'
.header
  position relative
  color #fff
  background-color  rgba(7,17,27,0.5)
  overflow hidden
  .content-wrapper
    padding 24px 12px 18px 24px
    font-size 0
    position relative   
    & > div
      display inline-block
      vertical-align top
    .avatar
      display inline-block
      img
         border-radius 2px
    p,span
      display inline-block
    .content
      margin-left 16px
      .title
        margin 2px 0 8px 0
        span
          width 30px
          height 18px
          bg-img("brand")
          background-size 30px 18px
          background-repeat no-repeat
        p
          font-size 16px
          line-height 18px
          font-weight bold
          vertical-align top
          margin-left 6px
      .delivery
        margin 8px 0 10px 0
        font-size 12px
      .activity
        p
          font-size 10px
          vertical-align middle
        .icon
          vertical-align middle
          display inline-block
          width 12px
          height 12px
          margin-right 4px
          background-size 12px 12px
          background-repeat no-repeat
          &.decrease
            bg-img("decrease_1")
          &.discount
            bg-img("discount_1")
          &.guarantee
            bg-img("guarantee_1")
          &.invoice
            bg-img("invoice_1")
          &.special
            bg-img("special_1")
      .activity-num
        font-size 10px
        position absolute
        right 12px
        bottom 14px
        line-height 24px
        padding 0 8px
        height 24px
        background-color rgba(0,0,0,0.2)
        text-align center
        border-radius 14px
        .font-icon 
          margin-left 4px
          transform rotate(180deg)
          -webkit-transform rotate(180deg)
  .bulletin-wrapper
    position relative
    height 28px
    line-height 28px
    background-color  rgba(7,17,27,0.2)
    padding 0 42px 0 12px
    white-space nowrap
    overflow hidden
    text-overflow ellipsis
    p,span
       display inline-block
    .icon  
      width 22px
      height 12px
      bg-img("bulletin")
      background-size 22px 12px
      background-repeat no-repeat
    p
      font-size 10px
      vertical-align top
      margin 0 4px 0 4px
    .font-icon
      position absolute
      right 12px
      font-size 10px
      bottom 10px 
      transform rotate(180deg)
      -webkit-transform rotate(180deg)
  .backgrund
    width 100%
    height 100%
    position absolute
    left 0
    top 0
    z-index -1    
    filter blur(10px)
  .detail 
    position fixed
    z-index 100
    width 100%
    height 100%
    top 0
    left 0
    background-color rgba(7,17,27,0.8)
    &.fade-enter-active 
      transition all .5s ease
    &.fade-leave-active
      transition all .5s ease
    &.fade-enter
      opacity 0
      transform translateY(-500px)
    &.fade-leave-to
      opacity 0
      transform translateY(-500px)
    .detail-wrapper
      min-height 100%
      .detall-main
        margin-top 64px
        padding-bottom 64px
        text-align center
        .name
          font-size 16px
          line-height 16px
          font-weight 700
        .vstar
          margin-top 16px
          margin-bottom 28px
        .title
          width 80%
          display flex
          margin  0 auto
          .line
            position relative
            flex 1
            top -6px
            border-bottom 1px solid rgba(255,255,255,0.2)
          .text
            font-weight 700
            padding 0 12px
            font-size 14px
        .supportss 
          width 80%
          text-align left
          margin 24px auto 28px auto
          & > li
            padding 0 12px
            margin-bottom 12px
            font-size 0
            &:last-child
              margin-bottom 0
            p
              display inline-block
              font-size 12px
              font-weight 200
              line-height 12px
            .icon
              display inline-block
              width 16px
              height 16px
              margin-right 1px
              background-size 16px 16px
              vertical-align top  
        .bull       
          width 80%
          margin  0 auto
          margin-top 24px
          padding  0 12px
          p
            font-size 12px
            font-weight 200
            line-height 18px
            letter-spacing:2px;  
            text-align left
    .detail-close
      position relative
      width 34px
      height 34px
      clear both
      margin -128px auto 0 auto
    
        

  

</style>