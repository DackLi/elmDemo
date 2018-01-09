<!--  -->
<template>
  <div class='star' :class="starType">
      <span v-for="item in itemList" :class="item" class="star-icon"></span>
  </div>
</template>

<script type='text/ecmascript-6'>
const LENGTH = 5
const CLS_ON = 'on'
const CLS_HALF = 'half'
const CLS_OFF = 'off'
export default {
  props: {
    size: {
      type: Number
    },
    score: {
      type: Number
    }
  },
  computed: {
    starType () {
      return 'star-' + this.size
    },
    itemList () {
      let list = []
      let score = Math.floor(this.score * 2) / 2
      let hasdecimal = score % 1 !== 0
      let integer = Math.floor(score)
      for (let i = 0; i < integer; i++) {
        list.push(CLS_ON)
      }
      if (hasdecimal) {
        list.push(CLS_HALF)
      }
      while (list.length < LENGTH) {
        list.push(CLS_OFF)
      }
      return list
    }
  },
  data () {
    return {}
  }
}
</script>
<style lang='stylus' rel='stylesheet/stylus' scoped>
@import '../../common/stylus/mixin.styl';

.star {
  font-size: 0;

  &.star-24 {
    .star-icon {
      width: 10px;
      height: 10px;
      margin-right: 3px;
      background-size: 10px 10px;
      background-repeat: no-repeat;
      display: inline-block;

      &:last-child {
        margin-right: 0;
      }

      &.on {
        bg-img('star24_on');
      }

      &.half {
        bg-img('star24_half');
      }

      &.off {
        bg-img('star24_off');
      }
    }
  }

  &.star-36 {
    .star-icon {
      width: 15px;
      height: 15px;
      margin-right: 6px;
      background-size: 15px 15px;
      background-repeat: no-repeat;
      display: inline-block;

      &:last-child {
        margin-right: 0;
      }

      &.on {
        bg-img('star36_on');
      }

      &.half {
        bg-img('star36_half');
      }

      &.off {
        bg-img('star36_off');
      }
    }
  }

  &.star-48 {
    .star-icon {
      width: 20px;
      height: 20px;
      margin-right: 22px;
      background-size: 22px 22px;
      background-repeat: no-repeat;
      display: inline-block;

      &:last-child {
        margin-right: 0;
      }

      &.on {
        bg-img('star48_on');
      }

      &.half {
        bg-img('star48_half');
      }

      &.off {
        bg-img('star48_off');
      }
    }
  }
}
</style>