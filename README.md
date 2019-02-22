# vue-scroll-box
在vue项目中使用，作为一个去滚动条的 可滚动的容器；

### 功能：
- 去除滚动条

- 可做上拉刷新等操作


### scroll_box.vue 使用示例：

````
<template lang="jade">
    //- 注：.book-list 要规定高度，才会有 scroll效果
    .book-list(ref="bookList")
          VerticalScroll(@whetherBottom="doSomething", :isShowBottomWapper="showPullUpWapper")
            template(slot="main")
              ul
                li.book-item(v-for="(item, index) in bookList", :key="index")
                  bookItem(:bookItemData="item")
            template(slot="footer")
              p 正在加载...

</template>

<script>
    import VerticalScroll from './scroll_box.vue'
    export default {
        components: {
            VerticalScroll
        },
        data () {
            return {
                showPullUpWapper: false, // 是否显示footer
            }
        },
        method: {
            // 列表处在最底部时，要做的事情
            doSomething(val) {
              this.showPullUpWapper = val
              if (val) {
                setTimeout(() => {
                  this.loadPageBooks() // 加载books
                }, 1000)
              }
            }
        }
    
    
    }
</script>

````

### 注意事项：
- .book-list 一定要设置高度，因为 scroll_box 是根据父元素的高度, 设置自己的高度的

-  `:isShowBottomWapper` 变量是暴露给用户的，让用户去手动设置 是否显示footer

-  `@whetherBottom` 是暴露给用户的一个方法，这个方法会有一个参数，这个参数是一个 Boolean 值，告诉用户 当前是否到达了列表的最底部


