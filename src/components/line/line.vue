<template>
  <div class="title">
    <div class="line"></div>
    <div class="text">{{title}}</div>
    <div class="line"></div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
    props: {
      title: {
        type: String,
        default: '优惠信息'
      }
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  .title
    display: flex
    width: 80%
    margin: 28px auto 24px auto
    .line
      flex: auto
      position: relative
      top: -6px
      border-bottom: 1px solid rgba(255, 255, 255, 0.2)
    .text
      padding: 0 12px
      font-size: 14px
      line-height 14px
      font-weight: 700
</style>
