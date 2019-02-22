<template lang="jade">
  .scroll-box
    .inner-box(ref="innerBox", :class="{'pullUpAnimation': isBottom}", @scroll="verticalScroll")
      slot(name="main")
    transition(name="fade")
      .pullup-wapper(v-show="isShowBottomWapper && bottomWrapperAvalilable")
        slot(name="footer")
          p 上拉刷新
</template>
<script>
export default {
  props: {
    // 外部应用传入的值, 控制是否显示 
    isShowBottomWapper: {
      type: Boolean,
      default: true
    },
  },
  data () {
    return {
      innerBoxEle: null, // scroll-box 中的元素
      isBottom: true, // 是否滚动到底部的 Boolean 值
      bottomWrapperAvalilable: false, // 是否显示 bottom wapper
    }
  },
  watch: {},
  updated () {
    if (this.isBottom != this.isGetBottom()) {
      this.isBottom = this.isGetBottom()
      this.emitWhetherBottom(this.isBottom)
    }
  },
  mounted () {
    // 挂载之后 获取到，.inner-box元素，此时内部元素已经准备完毕，可做后续.inner-box 的内容高度 及 滚动操作等
    this.innerBoxEle = this.$refs.innerBox
  },
  methods: {
    // FUNC: 元素滚动
    verticalScroll() {
      if (this.isBottom != this.isGetBottom()) {
        this.isBottom = this.isGetBottom()
        this.emitWhetherBottom(this.isBottom)
      }
      
    },
    // UNIT: 得到 isBottom 值
    isGetBottom () {
      let flag = false
      if (!!this.innerBoxEle) {
        if (this.innerBoxEle.scrollHeight === this.innerBoxEle.clientHeight && this.innerBoxEle.scrollTop === 0) {
          this.bottomWrapperAvalilable = false
        } else {
          this.bottomWrapperAvalilable = true
        }

        if (this.innerBoxEle.scrollHeight === this.innerBoxEle.clientHeight + this.innerBoxEle.scrollTop) {
          flag = true
        } else {
          flag = false
        }
      }
      return flag
    },
    // FUNC: 向外部提供 whetherBottom 方法，通知是否已到达 底部，从而可以做一些其它处理
    emitWhetherBottom (val) {
      this.$emit('whetherBottom', val)
    }
  }

}
</script>
<style lang="stylus" scoped>
  .scroll-box
    position relative
    height 100%
    overflow hidden
    .inner-box
      position relative
      width 100%
      height 100%
      box-sizing content-box
      padding-right 50px
      overflow-x hidden
      overflow-y scroll
      &.pullUpAnimation
        animation pullUpAnimation 1s linear
    .pullup-wapper
      position absolute
      z-index 99
      bottom 0
      width 100%
      height 20px
      line-height 20px
      text-align center
  
  .fade-enter-active
    animation fade .5s
  .fade-leave-active
    animation fade .5s reverse

  @keyframes pullUpAnimation
    0%
      transform translateY(0)
    10%
      transform translateY(-10px)
    20%
      transform translateY(-15px)
    50%
      transform translateY(-20px)
    80%
      transform translateY(-15px)
    90%
      transform translateY(-10px)
    100%
      transform translateY(0)

  @keyframes fade
    from
      opacity 0
      display none
    to
      opacity 1
      display block

</style>

