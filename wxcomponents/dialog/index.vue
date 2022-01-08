<template>
<uni-shadow-root class="dialog-index"><van-popup :show="show" transition="scale" :z-index="zIndex" :overlay="overlay" custom-class="van-dialog" :close-on-click-overlay="closeOnClickOverlay" @close="onClickOverlay">
  <view v-if="title" :class="'van-dialog__header '+(message || useSlot ? '' : 'van-dialog--isolated')">
    {{ title }}
  </view>

  <slot v-if="useSlot"></slot>
  <view v-else-if="message" :class="'van-dialog__message '+(title ? 'van-dialog__message--has-title' : '')">
    <text>{{ message }}</text>
  </view>

  <view class="van-hairline--top van-dialog__footer">
    <van-button v-if="showCancelButton" size="large" :loading="loading.cancel" class="van-dialog__button van-hairline--right" custom-class="van-dialog__cancel" @click="onCancel">
      {{ cancelButtonText }}
    </van-button>
    <van-button v-if="showConfirmButton" size="large" class="van-dialog__button" :loading="loading.confirm" custom-class="van-dialog__confirm" :open-type="confirmButtonOpenType" @click="onConfirm" @error="bindError" @contact="bindContact" @opensetting="bindOpenSetting" @getuserinfo="bindGetUserInfo" @getphonenumber="bindGetPhoneNumber">
      {{ confirmButtonText }}
    </van-button>
  </view>
</van-popup></uni-shadow-root>
</template>

<script>
import VanPopup from '../popup/index.vue'
import VanButton from '../button/index.vue'
global['__wxVueOptions'] = {components:{'van-popup': VanPopup,'van-button': VanButton}}

global['__wxRoute'] = 'dialog/index'
import { VantComponent } from '../common/component';
import { openType } from '../mixins/open-type';
VantComponent({
  mixins: [openType],
  props: {
    show: Boolean,
    title: String,
    message: String,
    useSlot: Boolean,
    asyncClose: Boolean,
    showCancelButton: Boolean,
    closeOnClickOverlay: Boolean,
    confirmButtonOpenType: String,
    zIndex: {
      type: Number,
      value: 100
    },
    confirmButtonText: {
      type: String,
      value: '确认'
    },
    cancelButtonText: {
      type: String,
      value: '取消'
    },
    showConfirmButton: {
      type: Boolean,
      value: true
    },
    overlay: {
      type: Boolean,
      value: true
    }
  },
  data: {
    loading: {
      confirm: false,
      cancel: false
    }
  },
  watch: {
    show: function show(_show) {
      !_show && this.stopLoading();
    }
  },
  methods: {
    onConfirm: function onConfirm() {
      this.handleAction('confirm');
    },
    onCancel: function onCancel() {
      this.handleAction('cancel');
    },
    onClickOverlay: function onClickOverlay() {
      this.onClose('overlay');
    },
    handleAction: function handleAction(action) {
      if (this.data.asyncClose) {
        this.setData({
          ["loading." + action]: true
        });
      }

      this.onClose(action);
    },
    close: function close() {
      this.setData({
        show: false
      });
    },
    stopLoading: function stopLoading() {
      this.setData({
        loading: {
          confirm: false,
          cancel: false
        }
      });
    },
    onClose: function onClose(action) {
      if (!this.data.asyncClose) {
        this.close();
      }

      this.$emit('close', action);
      this.$emit(action);
      var callback = this.data[action === 'confirm' ? 'onConfirm' : 'onCancel'];

      if (callback) {
        callback(this);
      }
    }
  }
});
export default global['__wxComponents']['dialog/index']
</script>
<style platform="mp-weixin">
@import '../common/index.css';.van-dialog{width:85%;font-size:16px;overflow:hidden;border-radius:4px;background-color:#fff}.van-dialog__header{font-weight:500;padding-top:25px;text-align:center}.van-dialog__header--isolated{padding:25px 0}.van-dialog__message{padding:25px;font-size:14px;line-height:1.5}.van-dialog__message--has-title{padding-top:12px;color:#666}.van-dialog__footer{display:-webkit-flex;display:flex}.van-dialog__button{-webkit-flex:1;flex:1}.van-dialog__cancel,.van-dialog__confirm{border:0!important}.van-dialog__confirm,.van-dialog__confirm:active{color:#1989fa!important}.van-dialog-bounce-enter{opacity:0;-webkit-transform:translate3d(-50%,-50%,0) scale(.7);transform:translate3d(-50%,-50%,0) scale(.7)}.van-dialog-bounce-leave-active{opacity:0;-webkit-transform:translate3d(-50%,-50%,0) scale(.9);transform:translate3d(-50%,-50%,0) scale(.9)}
</style>