<template>
<uni-shadow-root class="badge-index"><view :class="'van-badge van-hairline custom-class '+(active ? 'van-badge--active' : '')" @click="onClick">
  <view v-if="info" class="van-badge__info">{{ info }}</view>
  {{ title }}
</view></uni-shadow-root>
</template>

<script>

global['__wxRoute'] = 'badge/index'
import { VantComponent } from '../common/component';
VantComponent({
  relation: {
    type: 'ancestor',
    name: 'badge-group'
  },
  props: {
    info: Number,
    title: String
  },
  methods: {
    onClick: function onClick() {
      var group = this.getRelationNodes('../badge-group/index')[0];

      if (group) {
        group.setActive(this);
      }
    },
    setActive: function setActive(active) {
      this.setData({
        active: active
      });
    }
  }
});
export default global['__wxComponents']['badge/index']
</script>
<style platform="mp-weixin">
@import '../common/index.css';.van-badge{display:block;overflow:hidden;font-size:14px;line-height:1.4;-webkit-user-select:none;user-select:none;color:#666;word-break:break-all;box-sizing:border-box;padding:20px 12px 20px 9px;background-color:#f8f8f8;border-left:3px solid transparent}.van-badge:active{background-color:#e8e8e8}.van-badge::after{border-bottom-width:1px}.van-badge--active{font-weight:700;color:#333;border-color:#f44}.van-badge--active::after{border-right-width:1px}.van-badge--active,.van-badge--active:active{background-color:#fff}.van-badge__info{position:absolute;top:4px;right:2px;color:#fff;font-size:12px;font-weight:500;-webkit-transform:scale(.8);transform:scale(.8);text-align:center;box-sizing:border-box;padding:0 6px;min-width:18px;line-height:18px;border-radius:9px;background-color:#f44}
</style>