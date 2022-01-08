<template>
<uni-shadow-root class="switch-index"><view :style="style" :class="classes" @click="onClick">
  <view class="van-switch__node node-class">
    <van-loading v-if="loading" size="50%" custom-class="van-switch__loading"></van-loading>
  </view>
</view></uni-shadow-root>
</template>

<script>
import VanLoading from '../loading/index.vue'
global['__wxVueOptions'] = {components:{'van-loading': VanLoading}}

global['__wxRoute'] = 'switch/index'
import { VantComponent } from '../common/component';
VantComponent({
  field: true,
  classes: ['node-class'],
  props: {
    checked: Boolean,
    loading: Boolean,
    disabled: Boolean,
    activeColor: String,
    inactiveColor: String,
    size: {
      type: String,
      value: '30px'
    }
  },
  watch: {
    checked: function checked(value) {
      this.setData({
        value: value
      });
    }
  },
  computed: {
    classes: function classes() {
      return this.classNames('custom-class', 'van-switch', {
        'van-switch--on': this.data.checked,
        'van-switch--disabled': this.data.disabled
      });
    },
    style: function style() {
      var backgroundColor = this.data.checked ? this.data.activeColor : this.data.inactiveColor;
      return "font-size: " + this.data.size + "; " + (backgroundColor ? "background-color: " + backgroundColor : '');
    }
  },
  created: function created() {
    this.setData({
      value: this.data.checked
    });
  },
  methods: {
    onClick: function onClick() {
      if (!this.data.disabled && !this.data.loading) {
        var checked = !this.data.checked;
        this.$emit('input', checked);
        this.$emit('change', checked);
      }
    }
  }
});
export default global['__wxComponents']['switch/index']
</script>
<style platform="mp-weixin">
@import '../common/index.css';.van-switch{height:1em;width:1.8em;display:inline-block;position:relative;border-radius:1em;box-sizing:content-box;border:1px solid rgba(0,0,0,.1);background-color:#fff;transition:background-color .3s}.van-switch__node{top:0;left:0;z-index:1;width:1em;height:1em;transition:.3s;position:absolute;border-radius:100%;background-color:#fff;box-shadow:0 3px 1px 0 rgba(0,0,0,.05),0 2px 2px 0 rgba(0,0,0,.1),0 3px 3px 0 rgba(0,0,0,.05)}.van-switch__loading{top:25%;left:25%;position:absolute!important}.van-switch--on{background-color:#1989fa}.van-switch--on .van-switch__node{-webkit-transform:translateX(.8em);transform:translateX(.8em)}.van-switch--disabled{opacity:.4}
</style>