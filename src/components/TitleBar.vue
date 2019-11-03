<template>
  <transition name="slow-down">
      <div class="title-wrap" v-show="isShow">
        <div class="lt">
          <i class="icon icon-back"></i>
        </div>
        <div class="rt">
          <i class="icon icon-cart"></i>
          <i class="icon icon-person"></i>
          <i class="icon icon-more"></i>
        </div>
      </div>
    </transition>
</template>

<script>
export default {
  name: '',
  props: {
    isShow: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {

    }
  },
  created () {},
  mounted () {},
  computed: {},
  methods: {},
  components: {},
  watch: {}
}

</script>
<style lang='scss' scoped>
@import '@/assets/styles/global.scss';
 .title-wrap{
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: px2rem(48);
    z-index: 101;
    display: flex;
    justify-content: space-between;
    align-items: center;
    background: #fff;
    box-shadow: 0 px2rem(8) px2rem(8) rgba(0,0,0,.15);
    .lt{
      display: flex;
      align-items: center;
      // @include center;
    }
    .rt{
      display: flex;
      align-items: center;
    }
  }
</style>
