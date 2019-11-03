<template>
  <div class="ebook">
    <title-bar :isShow="isShow"></title-bar>
    <div class="read-wrapper">
      <div id="read"></div>
      <div class="mask">
        <div class="lt" @click="prevPage"></div>
        <div class="center" @click="topbtomFun"></div>
        <div class="rt" @click="nextPage"></div>
      </div>
    </div>
    <menu-bar
    :fontSizeList="fontSizeList"
    ref="MenuBar" :isShow="isShow"
    :defaultFontSize="defaultFontSize"
    @setFontSize="setFontSize"></menu-bar>
  </div>
</template>

<script>
import Epub from 'epubjs'
import MenuBar from '@/components/MenuBar'
import TitleBar from '@/components/TitleBar'
const DOWNLOAD_URL = '/2018_Book_AgileProcessesInSoftwareEngine.epub'
export default {
  name: 'ebook',
  props: [],
  data () {
    return {
      // book: ''
      isShow: false,
      fontSizeList: [
        { fontSize: 12 },
        { fontSize: 14 },
        { fontSize: 16 },
        { fontSize: 18 },
        { fontSize: 20 },
        { fontSize: 22 },
        { fontSize: 24 }
      ],
      defaultFontSize: 16
    }
  },
  created () {},
  mounted () {
    this.showEpub()
  },
  computed: {},
  methods: {
    setFontSize (fontSize) {
      this.defaultFontSize = fontSize
      if (this.themes) {
        this.themes.fontSize(fontSize + 'px')
      }
    },
    topbtomFun () {
      this.isShow = !this.isShow
      if (!this.isShow) {
        this.$refs.MenuBar.hideSet()
      }
    },
    prevPage () {
      // Rendition.prev
      if (this.rendition) {
        this.rendition.prev()
      }
    },
    nextPage () {
      // Rendition.next
      if (this.rendition) {
        this.rendition.next()
      }
    },
    // 电子书的解析和渲染
    showEpub () {
      // 生成book
      this.book = new Epub(DOWNLOAD_URL)
      // 生成Rendition 通过book.renderTo
      this.rendition = this.book.renderTo('read', {
        width: window.innerWidth,
        height: window.innerHeight
      })
      // 通过rendition.display渲染电子书
      this.rendition.display()

      // 获取themes对象
      this.themes = this.rendition.themes
      // 设置默认字体
      this.setFontSize(this.defaultFontSize)
    }
  },
  components: {
    MenuBar,
    TitleBar
  },
  watch: {}
}

</script>
<style lang='scss' scoped>
@import './assets/styles/global.scss';

.ebook{
  position: relative;
  .read-wrapper{
    .read{

    }
    .mask{
      display: flex;
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: 100;
      .lt{
        flex: 0 0 px2rem(100);
      }
      .center{
        flex: 1;
      }
      .rt{
        flex: 0 0 px2rem(100);
      }
    }
  }
}
</style>
