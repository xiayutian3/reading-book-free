<template>
<div class="menu-wrap">
  <transition name="slow-up">
    <div class="menus" v-show="isShow" :class="{'close-shadow':ifSetShow || !isShow}">
      <div class="menu" @click="showSet(3)">
        <i class="icon icon-menu"></i>
      </div>
      <div class="menu"  @click="showSet(2)">
        <i class="icon icon-progress"></i>
      </div>
      <div class="menu"  @click="showSet(1)">
        <i class="icon icon-bright"></i>
      </div>
      <div class="menu" @click="showSet(0)">
        <i class="icon icon-a">A</i>
      </div>
    </div>
  </transition>
  <transition name="slow-up">
    <div class="set-wrapper" v-show="ifSetShow">
      <div class="set-font-size" v-if="showTag === 0">
        <div class="preview" :style="{fontSize:fontSizeList[0].fontSize + 'px',transform:'translateX('+tranX+'px)'}">A</div>
        <div class="select" ref="listoffsetWidth">
          <div class="select-wrap" v-for="(item,index) in fontSizeList" :key="index" @click="setFontSize(item.fontSize)">
            <div class="line"></div>
            <div class="point-wrapper">
              <div class="point" v-show="item.fontSize === defaultFontSize">
                <div class="small-point"></div>
              </div>
            </div>
            <div class="line"></div>
          </div>
        </div>
        <div class="preview" :style="{fontSize:fontSizeList[fontSizeList.length-1].fontSize + 'px',transform:'translateX('+-tranX+'px)'}">A</div>
      </div>
      <div class="set-theme" v-if="showTag === 1">
        <div class="set-theme-item" v-for="(item,index) in themeList" :key="index" @click="setTeme(index)">
          <div class="preview" :style="{background:item.style.body.background}" :class="{'no-border':item.style.body.background == '#fff'}"></div>
          <div class="text" :class="{seleted:index === defaultTeme}">{{item.name}}</div>
        </div>
      </div>
      <div class="set-progress" v-if="showTag === 2">
        <div class="progress-wrap">
          <!-- //**松手的事件 */ @change -->
          <!-- 拖动事件 @input -->
          <input class="progress" type="range"
                                  max="100"
                                  min="0"
                                  step="1"
                                  @change="onProgressChange($event.target.value)"
                                  @input="onProgressInput($event.target.value)"
                                  :value="progress"
                                  :disabled="!bookAvailable"
                                  ref="progress"
                                  >
          <div class="text-wrap">
            <span>{{bookAvailable?progress + '%' : '加载中...'}}</span>
          </div>
        </div>
      </div>
    </div>
  </transition>
  <!-- navigation是目录信息 -->
  <content-view :ifShowContent="ifShowContent"
                v-show="ifShowContent"
                :navigation="navigation"
                :bookAvailable="bookAvailable"
                @jumpTo="jumpTo"
                ></content-view>
  <transition name="fade">
    <div class="content-mask"
          v-show="ifShowContent"
          @click="hideContent"
    ></div>
  </transition>
</div>

</template>

<script>
import ContentView from '@/components/Content.vue'
export default {
  name: '',
  props: {
    navigation: {
      type: Object,
      default () {
        return {}
      }
    },
    bookAvailable: {
      type: Boolean
    },
    isShow: {
      type: Boolean,
      default: false
    },
    fontSizeList: {
      type: Array,
      default () {
        return []
      }
    },
    defaultFontSize: {
      type: Number,
      default: 16
    },
    themeList: {
      type: Array,
      default () {
        return []
      }
    },
    defaultTeme: {
      type: Number,
      default: 0
    }
  },
  data () {
    return {
      ifSetShow: false,
      transFlag: false,
      tranX: 0,
      showTag: 0,
      progress: 0,
      ifShowContent: false
    }
  },
  created () {},
  mounted () {},
  computed: {},
  methods: {
    hideContent () {
      this.ifShowContent = false
    },
    jumpTo (target) {
      this.$emit('jumpTo', target)
    },
    // 拖动进度条事件
    onProgressInput (progress) {
      this.progress = progress
      this.$refs.progress.style.backgroundSize = `${this.progress}% 100%`
    },
    // 进度条松开后的事件，根据进度条的数值跳转到指定位置
    onProgressChange (progress) {
      this.$emit('onProgressChange', progress)
    },
    setTeme (i) {
      this.$emit('setTheme', i)
    },
    setFontSize (fontSize) {
      this.$emit('setFontSize', fontSize)
    },
    showSet (index) {
      this.showTag = index
      // 将设置栏打开
      this.ifSetShow = true
      if (index === 3) {
        this.ifSetShow = false
        this.ifShowContent = true
      }
      if (index !== 0) {
        return
      }

      this.$nextTick(() => {
        this.transFun()
      })
    },
    hideSet () {
      this.ifSetShow = false
    },
    transFun () {
      if (!this.transFlag) {
        let listWith = this.$refs.listoffsetWidth.offsetWidth
        let perItemWitch = listWith / (this.fontSizeList.length)
        let tranX = perItemWitch / 4
        this.tranX = tranX
        this.transFlag = true
      }
    }
  },
  components: {
    ContentView
  },
  watch: {}
}

</script>
<style lang='scss' scoped>
@import '@/assets/styles/global.scss';

  .menu-wrap{
    .menus{
      position: absolute;
      bottom: 0;
      left: 0;
      width: 100%;
      height: px2rem(48);
      z-index: 101;
      display: flex;
      background: #fff;
      box-shadow: 0 px2rem(-8) px2rem(8) rgba(0,0,0,.15);
      &.close-shadow{
        box-shadow: none;
      }
      .menu{
        flex: 1;
        @include center;
      }
    }
    .set-wrapper{
      position: absolute;
      bottom: px2rem(48);
      left: 0;
      width: 100%;
      height: px2rem(60);
      background: white;
      box-shadow: 0 px2rem(-8) px2rem(8) rgba(0,0,0,.15);
      z-index: 100;
      .set-font-size{
        display: flex;
        height: 100%;
        .preview{
          flex: 0 0 px2rem(40);
          @include center;
        }
        .select{
           flex: 1;
           display: flex;
           align-items: center;
          .select-wrap{
            flex: 1;
            display: flex;
            align-items: center;
            &:first-child{
             .line{
               &:first-child{
                 border-top: none;
               }
             }
            }
            &:last-child{
             .line{
               &:last-child{
                 border-top: none;
               }
             }
            }
            .line{
              flex: 1;
              height: 0;
              border-top: px2rem(1) solid #ccc;
            }
            .point-wrapper{
              position: relative;
              flex: 0 0 0;
              width: 0;
              height: px2rem(7);
              border-left: px2rem(1) solid #ccc;
              .point{
                position: absolute;
                top: px2rem(-8);
                left: px2rem(-10);
                width: px2rem(20);
                height: px2rem(20);
                border-radius: 50%;
                background: #fff;
                box-shadow: 0 px2rem(4) px2rem(4) rgba(0,0,0,.15);
                border: px2rem(1) solid #ccc;
                @include center;
                .small-point{
                  position: absolute;
                  width: px2rem(5);
                  height: px2rem(5);
                  border-radius: 50%;
                  background: #000;
                }
              }
            }
          }
        }
      }
      .set-theme{
        display: flex;
        height: 100%;
        .set-theme-item{
          flex: 1;
          display: flex;
          flex-direction: column;
          padding: px2rem(5);
          box-sizing: border-box;
          .preview{
            &.no-border{
              border: 1px solid #ccc;
            }
            flex: 1;
          }
          .text{
            &.seleted{
              color: #333;
            }
            flex: 0 0 px2rem(30);
            font-size: px2rem(16);
            color: #ccc;
            text-align: center;
            line-height: px2rem(30);
          }
        }
      }
      .set-progress{
        position: relative;
        width: 100%;
        height: 100%;
        .progress-wrap{
          width: 100%;
          height: 100%;
          @include center;
          padding: 0 px2rem(30);
          box-sizing: border-box;
          flex-direction: column;
          .progress{
            width: 100%;
            -webkit-appearance: none;
            height: px2rem(2);
            background: -webkit-linear-gradient(#999,#999) no-repeat #ddd;
            background-size: 0 100%;
            &:focus{
              outline: none;
            }
            &::-webkit-slider-thumb{
              -webkit-appearance: none;
              height: px2rem(20);
              width: px2rem(20);
              border-radius: 50%;
              background: #fff;
              box-shadow: 0 4px 4px 0 rgba(0,0,0,.15);
              border: px2rem(1) solid #ddd;
            }
          }
          .text-wrap{
            padding-top: px2rem(10);
            font-size: px2rem(12);
          }
        }
      }
    }
    .content-mask{
      position: absolute;
      top: 0;
      left: 0;
      z-index: 101;
      display: flex;
      width: 100%;
      height: 100%;
      background: rgba(51,51,51,.8)
    }
  }
</style>
