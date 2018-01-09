<template>
  <span  class="icon"  :class="classmap[stype]"></span>
</template>

<script>
export default {
  props: {
    stype: {
      type: Number
    }
  },
  data () {
    return {}
  },
  created () {
    this.classmap = ['decrease', 'discount', 'guarantee', 'invoice', 'special']
  },
  components: {}
}
</script>

<style scoped rel="stylesheet/stylus" lang="stylus">
@import '../../common/stylus/mixin.styl'
.icon 
  &.decrease 
    bg-img('decrease_1')
  &.discount 
    bg-img('discount_1')
  &.guarantee 
    bg-img('guarantee_1')
  &.invoice 
    bg-img('invoice_1')
  &.special 
    bg-img('special_1')
</style>
