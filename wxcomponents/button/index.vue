<template>
<uni-shadow-root class="button-index"><button :id="id" :lang="lang" :class="classes" :open-type="openType" :session-from="sessionFrom" :app-parameter="appParameter" :send-message-img="sendMessageImg" :send-message-path="sendMessagePath" :show-message-card="showMessageCard" :send-message-title="sendMessageTitle" @click="onClick" @error="bindError" @contact="bindContact" @opensetting="bindOpenSetting" @getuserinfo="bindGetUserInfo" @getphonenumber="bindGetPhoneNumber">
  <van-loading v-if="loading" size="20px" custom-class="loading-class" :color="type === 'default' ? '#c9c9c9' : ''"></van-loading>
  <slot v-else></slot>
</button></uni-shadow-root>
</template>

<script>
import VanLoading from '../loading/index.vue'
global['__wxVueOptions'] = {components:{'van-loading': VanLoading}}

global['__wxRoute'] = 'button/index'
import { VantComponent } from '../common/component';
import { button } from '../mixins/button';
import { openType } from '../mixins/open-type';
VantComponent({
  classes: ['loading-class'],
  mixins: [button, openType],
  props: {
    plain: Boolean,
    block: Boolean,
    round: Boolean,
    square: Boolean,
    loading: Boolean,
    disabled: Boolean,
    type: {
      type: String,
      value: 'default'
    },
    size: {
      type: String,
      value: 'normal'
    }
  },
  computed: {
    classes: function classes() {
      var _this$data = this.data,
          type = _this$data.type,
          size = _this$data.size,
          block = _this$data.block,
          plain = _this$data.plain,
          round = _this$data.round,
          square = _this$data.square,
          loading = _this$data.loading,
          disabled = _this$data.disabled;
      return this.classNames('custom-class', 'van-button', "van-button--" + type, "van-button--" + size, {
        'van-button--block': block,
        'van-button--round': round,
        'van-button--plain': plain,
        'van-button--square': square,
        'van-button--loading': loading,
        'van-button--disabled': disabled,
        'van-button--unclickable': disabled || loading
      });
    }
  },
  methods: {
    onClick: function onClick() {
      if (!this.data.disabled && !this.data.loading) {
        this.$emit('click');
      }
    }
  }
});
export default global['__wxComponents']['button/index']
</script>
<style platform="mp-weixin">
@import '../common/index.css';.van-button{position:relative;padding:0;display:inline-block;height:44px;line-height:42px;border-radius:2px;box-sizing:border-box;font-size:16px;text-align:center;vertical-align:middle;-webkit-appearance:none;-webkit-text-size-adjust:100%}.van-button::after{content:" ";position:absolute;top:50%;left:50%;opacity:0;width:100%;height:100%;border:inherit;border-color:#000;background-color:#000;border-radius:inherit;-webkit-transform:translate(-50%,-50%);transform:translate(-50%,-50%)}.van-button:active::after{opacity:.15}.van-button--unclickable::after{display:none}.van-button--default{color:#333;background-color:#fff;border:1px solid #eee}.van-button--primary{color:#fff;background-color:#4b0;border:1px solid #4b0}.van-button--danger{color:#fff;background-color:#f44;border:1px solid #f44}.van-button--warning{color:#fff;background-color:#ff976a;border:1px solid #ff976a}.van-button--plain{background-color:#fff}.van-button--plain.van-button--primary{color:#4b0}.van-button--plain.van-button--danger{color:#f44}.van-button--plain.van-button--warning{color:#ff976a}.van-button--large{width:100%;height:50px;line-height:48px}.van-button--normal{padding:0 15px;font-size:14px}.van-button--small{height:30px;padding:0 8px;min-width:60px;font-size:12px;line-height:28px}.van-button--mini{display:inline-block;width:50px;height:22px;line-height:20px;font-size:10px}.van-button--mini+.van-button--mini{margin-left:5px}.van-button--block{width:100%;display:block}.van-button--round{border-radius:10em}.van-button--square{border-radius:0}.van-button--disabled{opacity:.5}
</style>