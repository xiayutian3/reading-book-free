<template>
<div class="menu-wrap">
  <transition name="slow-up">
    <div class="menus" v-show="isShow" :class="{'close-shadow':ifSetShow || !isShow}">
      <div class="menu">
        <i class="icon icon-menu"></i>
      </div>
      <div class="menu">
        <i class="icon icon-progress"></i>
      </div>
      <div class="menu">
        <i class="icon icon-bright"></i>
      </div>
      <div class="menu" @click="showSet">
        <i class="icon icon-a">A</i>
      </div>
    </div>
  </transition>
  <transition name="slow-up">
    <div class="set-wrapper" v-show="ifSetShow">
      <div class="set-font-size">
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
    </div>
  </transition>
</div>

</template>

<script>
export default {
  name: '',
  props: {
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
    }
  },
  data () {
    return {
      ifSetShow: false,
      transFlag: false,
      tranX: 0
    }
  },
  created () {},
  mounted () {},
  computed: {},
  methods: {
    setFontSize (fontSize) {
      this.$emit('setFontSize', fontSize)
    },
    showSet () {
      this.ifSetShow = true
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
  components: {},
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
    }
  }
</style>
