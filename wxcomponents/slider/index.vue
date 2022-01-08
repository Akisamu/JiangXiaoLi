<template>
<uni-shadow-root class="slider-index"><view :class="'custom-class van-slider '+(disabled ? 'van-slider--disabled' : '')" @click="onClick">
  <view class="van-slider__bar" :style="barStyle">
    <view class="van-slider__button" @touchstart="onTouchStart" @touchmove.stop.prevent="onTouchMove" @touchend="onTouchEnd" @touchcancel="onTouchEnd"></view>
  </view>
</view></uni-shadow-root>
</template>

<script>

global['__wxRoute'] = 'slider/index'
import { VantComponent } from '../common/component';
import { touch } from '../mixins/touch';
VantComponent({
  mixins: [touch],
  props: {
    disabled: Boolean,
    max: {
      type: Number,
      value: 100
    },
    min: {
      type: Number,
      value: 0
    },
    step: {
      type: Number,
      value: 1
    },
    value: {
      type: Number,
      value: 0
    },
    barHeight: {
      type: String,
      value: '2px'
    }
  },
  watch: {
    value: function value(_value) {
      this.updateValue(_value, false);
    }
  },
  created: function created() {
    this.updateValue(this.data.value);
  },
  methods: {
    onTouchStart: function onTouchStart(event) {
      if (this.data.disabled) return;
      this.touchStart(event);
      this.startValue = this.format(this.data.value);
    },
    onTouchMove: function onTouchMove(event) {
      var _this = this;

      if (this.data.disabled) return;
      this.touchMove(event);
      this.getRect('.van-slider').then(function (rect) {
        var diff = _this.deltaX / rect.width * 100;

        _this.updateValue(_this.startValue + diff);
      });
    },
    onTouchEnd: function onTouchEnd() {
      if (this.data.disabled) return;
      this.updateValue(this.data.value, true);
    },
    onClick: function onClick(event) {
      var _this2 = this;

      if (this.data.disabled) return;
      this.getRect(function (rect) {
        var value = (event.detail.x - rect.left) / rect.width * 100;

        _this2.updateValue(value, true);
      });
    },
    updateValue: function updateValue(value, end) {
      value = this.format(value);
      this.setData({
        value: value,
        barStyle: "width: " + value + "%; height: " + this.data.barHeight + ";"
      });

      if (end) {
        this.$emit('change', value);
      }
    },
    format: function format(value) {
      var _this$data = this.data,
          max = _this$data.max,
          min = _this$data.min,
          step = _this$data.step;
      return Math.round(Math.max(min, Math.min(value, max)) / step) * step;
    }
  }
});
export default global['__wxComponents']['slider/index']
</script>
<style platform="mp-weixin">
@import '../common/index.css';.van-slider{position:relative;border-radius:999px;background-color:#e5e5e5}.van-slider__bar{position:relative;border-radius:inherit;background-color:#1989fa}.van-slider__button{position:absolute;top:50%;right:0;width:20px;height:20px;border-radius:50%;background-color:#fff;-webkit-transform:translate3d(50%,-50%,0);transform:translate3d(50%,-50%,0);box-shadow:0 1px 2px rgba(0,0,0,.5)}.van-slider__button::after{content:'';position:absolute;width:200%;height:200%;top:-50%;left:-50%}.van-slider--disabled{opacity:.3}
</style>