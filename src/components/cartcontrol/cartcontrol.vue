<template>
  <div class="cartcontrol">
    <div class="cart-decrease icon-remove_circle_outline" @click="decreaseCart" v-show="food.count>0"></div>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click="addCart"></div>
  </div>
</template>

<script type="text/ecmascript-6">
  import Vue from 'vue';
  export default {
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      addCart() {
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1)
        } else {
          this.food.count++;
        }
        console.log(this.food.count)
      },
      decreaseCart() {
        this.food.count--;
      }
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import '../../common/stylus/base.styl'
  .cartcontrol
    font-size: 0;
    .cart-decrease
      display: inline-block
      padding: 6px
      opacity: 1
      font-size: 24px
      color: #00a0dc
    .cart-count
      display: inline-block
      vertical-align: top
      width: 12px
      padding-top: 6px
      line-height: 24px
      text-align: center
      font-size: 10px
      color: rgb(147, 153, 159)
    .cart-add
      display: inline-block
      font-size: 24px
      color: #00a0dc
      line-height: 24px
      padding: 6px
</style>
