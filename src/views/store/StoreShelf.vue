<template>
  <div class="store-shelf">
    <shelf-title></shelf-title>
    <scroll class="store-shelf-scroll-wrapper"
            :top="0"
            :bottom="scrollBottom"
            @onScroll="onScroll"
            ref="scroll">
      <shelf-search></shelf-search>
      <shelf-list></shelf-list>
      <shelf-footer></shelf-footer>
    </scroll>
  </div>
</template>

<script>
  import ShelfTitle from '../../components/shelf/ShelfTitle'
  import { storeShelfMixin } from '../../utils/mixin'
  import Scroll from '../../components/common/Scroll'
  import ShelfSearch from '../../components/shelf/ShelfSearch'
  import ShelfList from '../../components/shelf/ShelfList'
  import ShelfFooter from '../../components/shelf/ShelfFooter'
  import { shelf } from '../../api/store'
  import { appendAddToShelf } from '../../utils/store'

  export default {
    mixins: [storeShelfMixin],
    components: {
      Scroll,
      ShelfTitle,
      ShelfSearch,
      ShelfList,
      ShelfFooter
    },
    watch: {
      isEditMode (isEditMode) {
        this.scrollBottom = isEditMode ? 48 : 0
        this.$nextTick(() => {
          this.$refs.scroll.refresh()
        })
      }
    },
    data () {
      return {
        scrollBottom: 0
      }
    },
    methods: {
      onScroll (offsetY) {
        this.setOffsetY(offsetY)
      },
      getShelfList () {
        shelf().then(response => {
          if (response.status === 200 && response.data && response.data.bookList) {
            this.setShelfList(appendAddToShelf(response.data.bookList))
          }
        })
      }
    },
    mounted () {
      this.getShelfList()
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss" scoped>
  @import "../../assets/styles/global";

  .store-shelf {
    position: relative;
    z-index: 100;
    width: 100%;
    height: 100%;
    background: white;
    .store-shelf-scroll-wrapper {
      position: absolute;
      top: 0;
      left: 0;
      z-index: 101;
    }
  }
</style>
