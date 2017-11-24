<template>
  <span class="icon" :class="itemClass"></span>
</template>

<script type="text/ecmascript-6">
  export default {
    props: {
      itemType: {
        type: Number
      }
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    computed: {
      itemClass() {
        return this.classMap[this.itemType];
      }
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import '../../common/stylus/mixin.styl'
  .icon
    width: 12px
    height: 12px
    display: inline-block
    background-size: 12px 12px
    background-repeat: no-repeat
    vertical-align: top
    margin-right: 2px
    &.decrease
      bg-image('decrease_3')
    &.discount
      bg-image('discount_3')
    &.guarantee
      bg-image('guarantee_3')
    &.invoice
      bg-image('invoice_3')
    &.special
      bg-image('special_3')
</style>
