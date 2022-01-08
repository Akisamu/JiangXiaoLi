<template>
<uni-shadow-root class="switch-cell-index"><van-cell center :title="title" :border="border" custom-class="van-switch-cell">
  <van-switch :size="size" :checked="checked" :loading="loading" :disabled="disabled" custom-class="van-switch-cell__switch" @change="onChange"></van-switch>
</van-cell></uni-shadow-root>
</template>

<script>
import VanCell from '../cell/index.vue'
import VanSwitch from '../switch/index.vue'
global['__wxVueOptions'] = {components:{'van-cell': VanCell,'van-switch': VanSwitch}}

global['__wxRoute'] = 'switch-cell/index'
import { VantComponent } from '../common/component';
VantComponent({
  field: true,
  props: {
    title: String,
    border: Boolean,
    checked: Boolean,
    loading: Boolean,
    disabled: Boolean,
    size: {
      type: String,
      value: '26px'
    }
  },
  watch: {
    checked: function checked(value) {
      this.setData({
        value: value
      });
    }
  },
  created: function created() {
    this.setData({
      value: this.data.checked
    });
  },
  methods: {
    onChange: function onChange(event) {
      this.$emit('change', event.detail);
    }
  }
});
export default global['__wxComponents']['switch-cell/index']
</script>
<style platform="mp-weixin">
@import '../common/index.css';.van-switch-cell{padding-top:8px;padding-bottom:8px}.van-switch-cell__switch{vertical-align:middle}
</style>