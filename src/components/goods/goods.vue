<template>
  <div>
    <div class="goods">
      <div class="menu-wrapper" ref="menuWrapper">
        <ul>
          <li class="menu-item" v-for="(item,index) in goods"
              :class="{'current':currentIndex===index}"
              @click="selectMenu(index,$event)"
          >
          <span class="text  border-1px">
            <icon v-show="item.type>0" :itemType="item.type"></icon>{{item.name}}
          </span>
          </li>
        </ul>
      </div>
      <div class="foods-wrapper" ref="foodsWrapper">
        <ul>
          <li class="food-list" v-for="item in goods" ref="foodList">
            <h1 class="title">{{item.name}}</h1>
            <ul>
              <li class="food-item border-1px" v-for="food in item.foods">
                <div class="icon">
                  <img :src="food.icon" alt="" width="57" height="57">
                </div>
                <div class="content">
                  <h2 class="name">{{food.name}}</h2>
                  <p v-show="food.description" class="desc">{{food.description}}</p>
                  <div class="extra">
                    <span class="count">月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span>
                  </div>
                  <div class="price">
                    <span class="now"> ¥{{food.price}}</span><span class="old"
                                                                   v-show="food.oldPrice">{{food.oldPrice}}</span>
                  </div>
                  <div class="cartcontrol-wrapper">
                    <cartControl :food="food"></cartControl>
                  </div>
                </div>
              </li>
            </ul>
          </li>
        </ul>
      </div>
    </div>
    <shopCart :selectFoods="selectFoods"></shopCart>
  </div>
</template>
<script type="text/ecmascript-6">
  import icon from 'components/icon/icon'
  import BScroll from 'better-scroll'
  import shopCart from 'components/shopcart/shopcart'
  import cartControl from 'components/cartcontrol/cartcontrol'
  const ERR_OK = 0;
  export default{
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        goods: [],
        listHeight: [],
        scrollY: 0
      };
    },
    created() {
      this.$http.get('api/goods').then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          this.goods = response.data;
//          console.log(this.goods);
          this.$nextTick(() => {
            this._initScroll();
            this._calculateHeight()
          })
        }
      })
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    components: {
      icon,
      shopCart,
      cartControl
    },
    methods: {
      _initScroll() {
        this.meunScroll = new BScroll(this.$refs.menuWrapper, {
          click: true
        })
        this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
          click: true,
          probeType: 3
        })
        this.foodsScroll.on('scroll', (pos) => {
//          console.log('this.scrollY:' + this.scrollY)
          this.scrollY = Math.abs(Math.round(pos.y));
        })
      },
      _calculateHeight() {
        let foodList = this.$refs.foodList;
        let height = 0
        this.listHeight.push(height)
        for (let i = 0; i < foodList.length; i++) {
          height += foodList[i].clientHeight
//          console.log('height:' + height)
          this.listHeight.push(height)
        }
      },
      selectMenu(index) {
//        console.log(index)
        let foodList = this.$refs.foodList;
        let el = foodList[index];
        this.foodsScroll.scrollToElement(el, 300);
      }
    },
    computed: {
      currentIndex() {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i];
          let height2 = this.listHeight[i + 1];
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
//            console.log('i::' + i + ',this.scrollY:' + this.scrollY + ',height1:' + height1 + 'height2:' + height2)
            return i;
          }
        }
        return 0;
      },
      selectFoods() {
        let foods = [];
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count) {
              foods.push(food);
            }
          });
        });
        return foods;
      }
    }
  }
</script>
<style lang="stylus" rel="stylesheet/stylus">
  @import '../../common/stylus/mixin.styl'
  @import '../../common/stylus/base.styl'
  .goods
    position: absolute
    top: 174px
    bottom: 46px
    display: flex
    width: 100%
    overflow: hidden
    .menu-wrapper
      width: 80px
      flex: 0 0 80px
      background: #f3f5f7
      .menu-item
        display: table
        width: 56px
        height: 54px
        padding: 0 12px
        font-size 12px
        line-height: 14px
        &.current
          color: rgb(240, 20, 20)
          background: #fff
          .text
            border-none()
        .text
          display: table-cell
          vertical-align: middle
          border-1px(rgba(7, 17, 27, 0.1))

    .foods-wrapper
      flex: auto
      .title
        height: 26px
        line-height 26px
        background: #f3f5f7
        border-left: 2px solid #d9dde1
        padding-left: 14px
        color: rgb(147, 153, 159);
        font-size: 12px
      .food-item
        margin: 18px
        padding-bottom: 18px
        display: flex
        border-1px(rgba(7, 17, 27, 0.1))
        &:last-child
          border-none()
      .icon
        width: 57px
        padding-right: 10px
        flex: 0 0 57px
      .content
        flex: auto
        .name
          font-size: 14px
          line-height: 14px
          color: rgb(7, 17, 27)
        .desc
          font-size: 10px
          line-height: 12px
          padding-top: 8px
          color: rgb(147, 153, 159)
        .extra
          font-size: 10px
          line-height: 10px
          color: rgb(147, 153, 159)
          padding-top: 8px
          .count
            margin-right: 12px
        .price
          font-weight: normal
          line-height: 24px
          .now
            font-size: 14px
            color: #f01414
            margin-right: 12px
          .old
            text-decoration: line-through
            font-size: 10px
            color: #93999f
        .cartcontrol-wrapper
          position: absolute
          right: 0
          bottom: 0
</style>

