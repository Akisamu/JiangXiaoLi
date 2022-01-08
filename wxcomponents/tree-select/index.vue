<template>
<uni-shadow-root class="tree-select-index"><view class="van-tree-select" :style="'height: '+(mainHeight)+'px'">
  <scroll-view scroll-y class="van-tree-select__nav">
    <view v-for="(item,index) in (items)" :key="item.index" :class="'van-tree-select__nitem van-ellipsis '+(mainActiveIndex === index ? 'van-tree-select__nitem--active' : '')" :data-index="index" @click="onClickNav">
      {{ item.text }}
    </view>
  </scroll-view>
  <scroll-view scroll-y class="van-tree-select__content" :style="'height: '+(itemHeight)+'px'">
    <view v-for="(item,index) in (subItems)" :key="item.id" :class="'van-tree-select__item van-ellipsis '+(activeId === item.id ? 'van-tree-select__item--active' : '')" :data-item="item" @click="onSelectItem">
      {{ item.text }}
      <van-icon v-if="activeId === item.id" name="success" class="van-tree-select__selected"></van-icon>
    </view>
  </scroll-view>
</view></uni-shadow-root>
</template>

<script>
import VanIcon from '../icon/index.vue'
global['__wxVueOptions'] = {components:{'van-icon': VanIcon}}

global['__wxRoute'] = 'tree-select/index'
import { VantComponent } from '../common/component';
var ITEM_HEIGHT = 44;
VantComponent({
  props: {
    items: Array,
    mainActiveIndex: {
      type: Number,
      value: 0
    },
    activeId: {
      type: Number,
      value: 0
    },
    maxHeight: {
      type: Number,
      value: 300
    }
  },
  data: {
    subItems: [],
    mainHeight: 0,
    itemHeight: 0
  },
  watch: {
    items: function items() {
      this.updateSubItems();
      this.updateMainHeight();
    },
    maxHeight: function maxHeight() {
      this.updateItemHeight();
      this.updateMainHeight();
    },
    mainActiveIndex: 'updateSubItems'
  },
  methods: {
    // 当一个子项被选择时
    onSelectItem: function onSelectItem(event) {
      this.$emit('click-item', event.currentTarget.dataset.item);
    },
    // 当一个导航被点击时
    onClickNav: function onClickNav(event) {
      var index = event.currentTarget.dataset.index;
      this.$emit('click-nav', {
        index: index
      });
    },
    // 更新子项列表
    updateSubItems: function updateSubItems() {
      var selectedItem = this.data.items[this.data.mainActiveIndex] || {};
      this.setData({
        subItems: selectedItem.children || []
      });
      this.updateItemHeight();
    },
    // 更新组件整体高度，根据最大高度和当前组件需要展示的高度来决定
    updateMainHeight: function updateMainHeight() {
      var maxHeight = Math.max(this.data.items.length * ITEM_HEIGHT, this.data.subItems.length * ITEM_HEIGHT);
      this.setData({
        mainHeight: Math.min(maxHeight, this.data.maxHeight)
      });
    },
    // 更新子项列表高度，根据可展示的最大高度和当前子项列表的高度决定
    updateItemHeight: function updateItemHeight() {
      this.setData({
        itemHeight: Math.min(this.data.subItems.length * ITEM_HEIGHT, this.data.maxHeight)
      });
    }
  }
});
export default global['__wxComponents']['tree-select/index']
</script>
<style platform="mp-weixin">
@import '../common/index.css';.van-tree-select{-webkit-user-select:none;user-select:none;position:relative;font-size:16px}.van-tree-select__nav{width:35%;position:absolute;left:0;top:0;bottom:0;background-color:#fff}.van-tree-select__nitem{line-height:44px;padding:0 15px;background-color:#fff}.van-tree-select__nitem--active,.van-tree-select__nitem:active{background-color:#f8f8f8}.van-tree-select__nitem--active{font-weight:500}.van-tree-select__content{width:65%;padding:0 15px;margin-left:35%;box-sizing:border-box}.van-tree-select__item{position:relative;line-height:44px;padding-left:5px;padding-right:18px}.van-tree-select__item--active,.van-tree-select__item:active{color:#f44}.van-tree-select__selected{float:right;position:absolute;right:0;top:0;bottom:0;line-height:inherit}
</style>