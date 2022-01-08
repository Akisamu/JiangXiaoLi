<template>
<uni-shadow-root class="tabbar-item-index"><view :class="'van-tabbar-item '+(active ? 'van-tabbar-item--active' : '')" @click="onClick">
  <view :class="'van-tabbar-item__icon '+(dot ? 'van-tabbar-item__icon--dot' : '')">
    <van-icon v-if="icon" :name="icon" :info="info"></van-icon>
    <block v-else>
      <slot v-if="active" name="icon-active"></slot>
      <slot v-else name="icon"></slot>
      <view v-if="info !== null" class="van-icon__info">{{ info }}</view>
    </block>
  </view>
  <view class="van-tabbar-item__text">
    <slot></slot>
  </view>
</view></uni-shadow-root>
</template>

<script>
import VanIcon from '../icon/index.vue'
global['__wxVueOptions'] = {components:{'van-icon': VanIcon}}

global['__wxRoute'] = 'tabbar-item/index'
import { VantComponent } from '../common/component';
VantComponent({
  props: {
    info: null,
    icon: String,
    dot: Boolean
  },
  relation: {
    name: 'tabbar',
    type: 'ancestor'
  },
  data: {
    active: false
  },
  methods: {
    onClick: function onClick() {
      var parent = this.getRelationNodes('../tabbar/index')[0];

      if (parent) {
        parent.onChange(this);
      }

      this.$emit('click');
    },
    setActive: function setActive(active) {
      if (this.data.active !== active) {
        this.setData({
          active: active
        });
      }
    }
  }
});
export default global['__wxComponents']['tabbar-item/index']
</script>
<style platform="mp-weixin">
@import '../common/index.css';.tabbar-item-index{-webkit-flex:1;flex:1}.van-tabbar-item{color:#666;height:100%;display:-webkit-flex;display:flex;line-height:1;font-size:12px;-webkit-align-items:center;align-items:center;-webkit-flex-direction:column;flex-direction:column;-webkit-justify-content:center;justify-content:center}.van-tabbar-item__icon{font-size:18px;margin-bottom:5px;position:relative}.van-tabbar-item__icon .van-icon{display:block}.van-tabbar-item__icon .van-icon__info{color:#fff;left:100%;top:-.5em;font-size:.6em;padding:0 .25em;text-align:center;min-width:1.4em;line-height:1.4;position:absolute;border-radius:.6em;box-sizing:border-box;background-color:#f44;-webkit-transform:translateX(-50%);transform:translateX(-50%);font-family:PingFang SC,Helvetica Neue,Arial,sans-serif}.van-tabbar-item__icon--dot::after{top:0;right:-8px;width:8px;height:8px;content:' ';position:absolute;border-radius:100%;background-color:#f44}.van-tabbar-item__icon image{width:50px;height:18px}.van-tabbar-item--active{color:#1989fa}
</style>