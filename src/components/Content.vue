<template>
  <transition name="slide-right">
    <div class="content">
      <div class="content-wrap" v-if="ifShowContent">
        <div class="content-item" v-for="(item,i) in navigation.toc" :key="i"
        @click="jumpTo(item.href)">
          <span class="text">{{item.label}}</span>
        </div>
      </div>
      <div class="empty" v-else>加载中...</div>
    </div>
  </transition>
</template>

<script>
export default {
  name: 'read-content',
  props:
  {
    'bookAvailable': Boolean,
    'navigation': Object,
    'ifShowContent': Boolean
  },
  data () {
    return {

    }
  },
  created () {},
  mounted () {},
  computed: {},
  methods: {
    jumpTo (href) {
      this.$emit('jumpTo', href)
    }
  },
  components: {},
  watch: {}
}

</script>
<style lang='scss' scoped>
@import '@/assets/styles/global.scss';
.content{
   position: absolute;
   top: 0;
   left: 0;
   width: 80%;
   height: 100%;
   overflow-y: auto;
   z-index: 102;
   background: #fff;
  .content-wrap{
    .content-item{
      height: px2rem(40);
      line-height: px2rem(20);
      font-size: 12px;
      color: #ccc;
      .text{

      }
    }
  }
  .empty{

  }
}
</style>
