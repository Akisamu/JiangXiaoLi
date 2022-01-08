<template>
<uni-shadow-root class="toast-index"><van-overlay v-if="mask || forbidClick" :show="show" :mask="mask" :z-index="zIndex"></van-overlay>
<van-transition :show="show" :custom-style="'z-index: '+(zIndex)" custom-class="van-toast__container">
  <view :class="'van-toast van-toast--'+(type === 'text' ? 'text' : 'icon')+' van-toast--'+(position)" @touchmove.stop.prevent="noop">
    
    <text v-if="type === 'text'">{{ message }}</text>

    
    <block v-else>
      <van-loading v-if="type === 'loading'" color="white" :type="loadingType" custom-class="van-toast__loading"></van-loading>
      <van-icon v-else class="van-toast__icon" :name="type"></van-icon>
      <text v-if="message" class="van-toast__text">{{ message }}</text>
    </block>
  </view>
</van-transition></uni-shadow-root>
</template>

<script>
import VanIcon from '../icon/index.vue'
import VanLoading from '../loading/index.vue'
import VanOverlay from '../overlay/index.vue'
import VanTransition from '../transition/index.vue'
global['__wxVueOptions'] = {components:{'van-icon': VanIcon,'van-loading': VanLoading,'van-overlay': VanOverlay,'van-transition': VanTransition}}

global['__wxRoute'] = 'toast/index'
import { VantComponent } from '../common/component';
VantComponent({
  props: {
    show: Boolean,
    mask: Boolean,
    message: String,
    forbidClick: Boolean,
    zIndex: {
      type: Number,
      value: 1000
    },
    type: {
      type: String,
      value: 'text'
    },
    loadingType: {
      type: String,
      value: 'circular'
    },
    position: {
      type: String,
      value: 'middle'
    }
  },
  methods: {
    clear: function clear() {
      this.setData({
        show: false
      });
    },
    // for prevent touchmove
    noop: function noop() {}
  }
});
export default global['__wxComponents']['toast/index']
</script>
<style platform="mp-weixin">
@import '../common/index.css';.van-toast{display:-webkit-flex;display:flex;color:#fff;font-size:12px;line-height:1.2;border-radius:5px;word-break:break-all;-webkit-align-items:center;align-items:center;-webkit-justify-content:center;justify-content:center;-webkit-flex-direction:column;flex-direction:column;box-sizing:border-box;background-color:rgba(0,0,0,.7)}.van-toast__container{top:50%;left:50%;position:fixed;-webkit-transform:translate(-50%,-50%);transform:translate(-50%,-50%)}.van-toast--text{padding:12px;min-width:220px}.van-toast--icon{width:120px;min-height:120px;padding:15px}.van-toast--icon .van-toast__icon{height:1em;font-size:50px}.van-toast--icon .van-toast__text{font-size:14px;padding-top:10px}.van-toast__loading{margin:10px 0 5px}.van-toast--top{-webkit-transform:translate(0,-30vh);transform:translate(0,-30vh)}.van-toast--bottom{-webkit-transform:translate(0,30vh);transform:translate(0,30vh)}
</style>