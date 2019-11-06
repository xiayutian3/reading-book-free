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
    @setFontSize="setFontSize"
    :themeList="themesList"
    :defaultTeme="defaultTeme"
    @setTheme="setTheme"
    :bookAvailable="bookAvailable"
    @onProgressChange="onProgressChange"
    :navigation="navigation"
    @jumpTo ="jumpTo"
    ></menu-bar>
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
      defaultFontSize: 16,
      themesList: [
        {
          name: 'default',
          style: {
            body: {
              'color': '#000', 'background': '#fff'
            }
          }
        },
        {
          name: 'eye',
          style: {
            body: {
              'color': '#000', 'background': '#ceeaba'
            }
          }
        },
        {
          name: 'night',
          style: {
            body: {
              'color': '#fff', 'background': '#000'
            }
          }
        },
        {
          name: 'gold',
          style: {
            body: {
              'color': '#000', 'background': 'rgb(241,236,226)'
            }
          }
        }
      ],
      defaultTeme: 0,
      // 图书是否处于可用状态
      bookAvailable: false,
      navigation: {}
    }
  },
  created () {},
  mounted () {
    this.showEpub()
  },
  computed: {},
  methods: {

    // 根据目录连接跳转到指定位置
    jumpTo (href) {
      this.rendition.display(href)
      // 跳转后隐藏 头部 低部
      this.hideAll()
    },
    hideAll () {
      // 隐藏标题栏和菜单栏
      this.isShow = false
      // 隐藏菜单栏弹出的设置栏
      this.$refs.MenuBar.hideSet()
      // 隐藏目录
      this.$refs.MenuBar.hideContent()
    },
    // progress是数字 0 -- 100
    onProgressChange (progress) {
      const percentage = progress / 100
      const location = percentage > 0 ? this.locations.cfiFromPercentage(percentage) : 0
      this.rendition.display(location)
    },
    setTheme (index) {
      this.defaultTeme = index
      this.themes.select(this.themesList[index].name)
    },
    registerTheme () {
      this.themesList.forEach(theme => {
        this.themes.register(theme.name, theme.style)
      })
    },
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

      // this.themes.register(name,style)
      // this.themes.select(name)
      this.registerTheme()
      this.setTheme(this.defaultTeme)

      // 获取Locations对象 （跟进度条相关）
      // 通过epubjs的钩子函数来实现

      this.book.ready.then(() => {
        // 生成目录
        this.navigation = this.book.navigation
        // （跟进度条相关）
        return this.book.locations.generate()
      }).then(result => {
        this.locations = this.book.locations
        this.bookAvailable = true
      })
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
