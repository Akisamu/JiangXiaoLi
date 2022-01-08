<template>
<uni-shadow-root class="popup-index"><van-overlay v-if="inited && overlay" mask :show="show" :z-index="zIndex" :custom-style="overlayStyle" @click="onClickOverlay"></van-overlay>
<view v-if="inited" :class="'custom-class van-popup '+(position ? 'van-popup--' + position : '')" :style="'z-index: '+(zIndex)+'; animation-name: van-'+(transition || position)+'-'+(type)+'; animation-duration: '+(duration)+'ms; '+(display ? '' : 'display: none;')+(customStyle)" @animationend="onAnimationEnd">
  <slot></slot>
</view></uni-shadow-root>
</template>

<script>
import VanOverlay from '../overlay/index.vue'
global['__wxVueOptions'] = {components:{'van-overlay': VanOverlay}}

global['__wxRoute'] = 'popup/index'
import { VantComponent } from '../common/component';
import { transition } from '../mixins/transition';
VantComponent({
  mixins: [transition(false)],
  props: {
    transition: String,
    customStyle: String,
    overlayStyle: String,
    zIndex: {
      type: Number,
      value: 100
    },
    overlay: {
      type: Boolean,
      value: true
    },
    closeOnClickOverlay: {
      type: Boolean,
      value: true
    },
    position: {
      type: String,
      value: 'center'
    }
  },
  methods: {
    onClickOverlay: function onClickOverlay() {
      this.$emit('click-overlay');

      if (this.data.closeOnClickOverlay) {
        this.$emit('close');
      }
    }
  }
});
export default global['__wxComponents']['popup/index']
</script>
<style platform="mp-weixin">
@import '../common/index.css';.van-popup{top:50%;left:50%;position:fixed;max-height:100%;overflow-y:auto;box-sizing:border-box;background-color:#fff;-webkit-overflow-scrolling:touch;-webkit-animation:ease both;animation:ease both}.van-popup--center{-webkit-transform:translate3d(-50%,-50%,0);transform:translate3d(-50%,-50%,0)}.van-popup--top{width:100%;top:0;right:auto;bottom:auto;left:50%}.van-popup--right{top:50%;right:0;bottom:auto;left:auto}.van-popup--bottom{width:100%;top:auto;bottom:0;right:auto;left:50%}.van-popup--left{top:50%;right:auto;bottom:auto;left:0}@-webkit-keyframes van-center-enter{from{opacity:0}}@keyframes van-center-enter{from{opacity:0}}@-webkit-keyframes van-center-leave{to{opacity:0}}@keyframes van-center-leave{to{opacity:0}}@-webkit-keyframes van-scale-enter{from{opacity:0;-webkit-transform:translate3d(-50%,-50%,0) scale(.7);transform:translate3d(-50%,-50%,0) scale(.7)}}@keyframes van-scale-enter{from{opacity:0;-webkit-transform:translate3d(-50%,-50%,0) scale(.7);transform:translate3d(-50%,-50%,0) scale(.7)}}@-webkit-keyframes van-scale-leave{to{opacity:0;-webkit-transform:translate3d(-50%,-50%,0) scale(.7);transform:translate3d(-50%,-50%,0) scale(.7)}}@keyframes van-scale-leave{to{opacity:0;-webkit-transform:translate3d(-50%,-50%,0) scale(.7);transform:translate3d(-50%,-50%,0) scale(.7)}}@-webkit-keyframes van-bottom-enter{from{-webkit-transform:translate3d(-50%,100%,0);transform:translate3d(-50%,100%,0)}to{-webkit-transform:translate3d(-50%,0,0);transform:translate3d(-50%,0,0)}}@keyframes van-bottom-enter{from{-webkit-transform:translate3d(-50%,100%,0);transform:translate3d(-50%,100%,0)}to{-webkit-transform:translate3d(-50%,0,0);transform:translate3d(-50%,0,0)}}@-webkit-keyframes van-bottom-leave{from{-webkit-transform:translate3d(-50%,0,0);transform:translate3d(-50%,0,0)}to{-webkit-transform:translate3d(-50%,100%,0);transform:translate3d(-50%,100%,0)}}@keyframes van-bottom-leave{from{-webkit-transform:translate3d(-50%,0,0);transform:translate3d(-50%,0,0)}to{-webkit-transform:translate3d(-50%,100%,0);transform:translate3d(-50%,100%,0)}}@-webkit-keyframes van-top-enter{from{-webkit-transform:translate3d(-50%,-100%,0);transform:translate3d(-50%,-100%,0)}to{-webkit-transform:translate3d(-50%,0,0);transform:translate3d(-50%,0,0)}}@keyframes van-top-enter{from{-webkit-transform:translate3d(-50%,-100%,0);transform:translate3d(-50%,-100%,0)}to{-webkit-transform:translate3d(-50%,0,0);transform:translate3d(-50%,0,0)}}@-webkit-keyframes van-top-leave{from{-webkit-transform:translate3d(-50%,0,0);transform:translate3d(-50%,0,0)}to{-webkit-transform:translate3d(-50%,-100%,0);transform:translate3d(-50%,-100%,0)}}@keyframes van-top-leave{from{-webkit-transform:translate3d(-50%,0,0);transform:translate3d(-50%,0,0)}to{-webkit-transform:translate3d(-50%,-100%,0);transform:translate3d(-50%,-100%,0)}}@-webkit-keyframes van-left-enter{from{-webkit-transform:translate3d(-100%,-50%,0);transform:translate3d(-100%,-50%,0)}to{-webkit-transform:translate3d(0,-50%,0);transform:translate3d(0,-50%,0)}}@keyframes van-left-enter{from{-webkit-transform:translate3d(-100%,-50%,0);transform:translate3d(-100%,-50%,0)}to{-webkit-transform:translate3d(0,-50%,0);transform:translate3d(0,-50%,0)}}@-webkit-keyframes van-left-leave{from{-webkit-transform:translate3d(0,-50%,0);transform:translate3d(0,-50%,0)}to{-webkit-transform:translate3d(-100%,-50%,0);transform:translate3d(-100%,-50%,0)}}@keyframes van-left-leave{from{-webkit-transform:translate3d(0,-50%,0);transform:translate3d(0,-50%,0)}to{-webkit-transform:translate3d(-100%,-50%,0);transform:translate3d(-100%,-50%,0)}}@-webkit-keyframes van-right-enter{from{-webkit-transform:translate3d(100%,-50%,0);transform:translate3d(100%,-50%,0)}to{-webkit-transform:translate3d(0,-50%,0);transform:translate3d(0,-50%,0)}}@keyframes van-right-enter{from{-webkit-transform:translate3d(100%,-50%,0);transform:translate3d(100%,-50%,0)}to{-webkit-transform:translate3d(0,-50%,0);transform:translate3d(0,-50%,0)}}@-webkit-keyframes van-right-leave{from{-webkit-transform:translate3d(0,-50%,0);transform:translate3d(0,-50%,0)}to{-webkit-transform:translate3d(100%,-50%,0);transform:translate3d(100%,-50%,0)}}@keyframes van-right-leave{from{-webkit-transform:translate3d(0,-50%,0);transform:translate3d(0,-50%,0)}to{-webkit-transform:translate3d(100%,-50%,0);transform:translate3d(100%,-50%,0)}}
</style>