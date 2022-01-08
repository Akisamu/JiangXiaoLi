<template>
<uni-shadow-root class="tabs-index"><view :class="'custom-class van-tabs van-tabs--'+(type)">
  <view :style="'z-index: '+(zIndex)" :class="'van-tabs__wrap '+(scrollable ? 'van-tabs__wrap--scrollable' : '')+' '+(type === 'line' && border ? 'van-hairline--top-bottom' : '')">
    <scroll-view :scroll-x="scrollable" scroll-with-animation :scroll-left="scrollLeft" :class="'van-tabs__scroll--'+(type)" :style="color ? 'border-color: ' + color : ''">
      <view :class="'van-tabs__nav van-tabs__nav--'+(type)">
        <view v-if="type === 'line'" class="van-tabs__line" :style="lineStyle"></view>
        <view v-for="(item,index) in (tabs)" :key="item.index" :data-index="index" :class="'van-ellipsis van-tab '+(index === active ? 'van-tab--active' : '')+' '+(item.data.disabled ? 'van-tab--disabled' : '')" :style="(color && (index === active) !== (type === 'card') && !item.data.disabled ? 'color: ' + color : '')+' '+(color && index === active && type === 'card' ? ';background-color:' + color : '')+' '+(color ? ';border-color: ' + color : '')" @click="onTap">
          {{ item.data.title }}
        </view>
      </view>
    </scroll-view>
  </view>
  <slot></slot>
</view></uni-shadow-root>
</template>

<script>

global['__wxRoute'] = 'tabs/index'
import { VantComponent } from '../common/component';
VantComponent({
  relation: {
    name: 'tab',
    type: 'descendant',
    linked: function linked(child) {
      this.data.tabs.push({
        instance: child,
        data: child.data
      });
      this.updateTabs();
    },
    unlinked: function unlinked(child) {
      var tabs = this.data.tabs.filter(function (item) {
        return item.instance !== child;
      });
      this.setData({
        tabs: tabs,
        scrollable: tabs.length > this.data.swipeThreshold
      });
      this.setActiveTab();
    }
  },
  props: {
    color: String,
    lineWidth: Number,
    active: {
      type: Number,
      value: 0
    },
    type: {
      type: String,
      value: 'line'
    },
    border: {
      type: Boolean,
      value: true
    },
    duration: {
      type: Number,
      value: 0.2
    },
    zIndex: {
      type: Number,
      value: 1
    },
    swipeThreshold: {
      type: Number,
      value: 4
    }
  },
  data: {
    tabs: [],
    lineStyle: '',
    scrollLeft: 0,
    scrollable: false
  },
  watch: {
    swipeThreshold: function swipeThreshold() {
      this.setData({
        scrollable: this.data.tabs.length > this.data.swipeThreshold
      });
    },
    color: 'setLine',
    lineWidth: 'setLine',
    active: 'setActiveTab'
  },
  mounted: function mounted() {
    this.setLine();
    this.scrollIntoView();
  },
  methods: {
    updateTabs: function updateTabs() {
      var tabs = this.data.tabs;
      this.setData({
        tabs: tabs,
        scrollable: tabs.length > this.data.swipeThreshold
      });
      this.setActiveTab();
    },
    trigger: function trigger(eventName, index) {
      this.$emit(eventName, {
        index: index,
        title: this.data.tabs[index].data.title
      });
    },
    onTap: function onTap(event) {
      var index = event.currentTarget.dataset.index;

      if (this.data.tabs[index].data.disabled) {
        this.trigger('disabled', index);
      } else {
        this.trigger('click', index);
        this.setActive(index);
      }
    },
    setActive: function setActive(active) {
      if (active !== this.data.active) {
        this.trigger('change', active);
        this.setData({
          active: active
        });
        this.setActiveTab();
      }
    },
    setLine: function setLine() {
      var _this = this;

      if (this.data.type !== 'line') {
        return;
      }

      this.getRect('.van-tab', true).then(function (rects) {
        var rect = rects[_this.data.active];
        var width = _this.data.lineWidth || rect.width / 2;
        var left = rects.slice(0, _this.data.active).reduce(function (prev, curr) {
          return prev + curr.width;
        }, 0);
        left += (rect.width - width) / 2;

        _this.setData({
          lineStyle: "\n            width: " + width + "px;\n            background-color: " + _this.data.color + ";\n            transform: translateX(" + left + "px);\n            transition-duration: " + _this.data.duration + "s;\n          "
        });
      });
    },
    setActiveTab: function setActiveTab() {
      var _this2 = this;

      this.data.tabs.forEach(function (item, index) {
        var data = {
          active: index === _this2.data.active
        };

        if (data.active) {
          data.inited = true;
        }

        if (data.active !== item.instance.data.active) {
          item.instance.setData(data);
        }
      });
      this.setData({}, function () {
        _this2.setLine();

        _this2.scrollIntoView();
      });
    },
    // scroll active tab into view
    scrollIntoView: function scrollIntoView() {
      var _this3 = this;

      if (!this.data.scrollable) {
        return;
      }

      this.getRect('.van-tab', true).then(function (tabRects) {
        var tabRect = tabRects[_this3.data.active];
        var offsetLeft = tabRects.slice(0, _this3.data.active).reduce(function (prev, curr) {
          return prev + curr.width;
        }, 0);
        var tabWidth = tabRect.width;

        _this3.getRect('.van-tabs__nav').then(function (navRect) {
          var navWidth = navRect.width;

          _this3.setData({
            scrollLeft: offsetLeft - (navWidth - tabWidth) / 2
          });
        });
      });
    }
  }
});
export default global['__wxComponents']['tabs/index']
</script>
<style platform="mp-weixin">
@import '../common/index.css';.van-tabs{position:relative;-webkit-tap-highlight-color:transparent}.van-tabs__wrap{top:0;left:0;right:0;position:absolute}.van-tabs__wrap--page-top{position:fixed}.van-tabs__wrap--content-bottom{top:auto;bottom:0}.van-tabs__wrap--scrollable .van-tab{-webkit-flex:0 0 22%;flex:0 0 22%}.van-tabs__scroll--card{border:1px solid #f44;border-radius:2px}.van-tabs__nav{display:-webkit-flex;display:flex;-webkit-user-select:none;user-select:none;position:relative;background-color:#fff}.van-tabs__nav--line{height:100%}.van-tabs__nav--card{height:30px}.van-tabs__nav--card .van-tab{color:#f44;border-right:1px solid #f44;line-height:30px}.van-tabs__nav--card .van-tab:last-child{border-right:none}.van-tabs__nav--card .van-tab.van-tab--active{color:#fff;background-color:#f44}.van-tabs__line{z-index:1;left:0;bottom:0;height:2px;position:absolute;background-color:#f44;border-radius:2px 0 2px 0}.van-tabs--line{padding-top:44px}.van-tabs--line .van-tabs__wrap{height:44px}.van-tabs--card{padding-top:30px;margin:0 15px}.van-tabs--card .van-tabs__wrap{height:30px}.van-tab{-webkit-flex:1;flex:1;cursor:pointer;padding:0 5px;font-size:14px;position:relative;color:#333;line-height:44px;text-align:center;box-sizing:border-box;background-color:#fff;min-width:0}.van-tab span{display:block}.van-tab--active{color:#f44}.van-tab--disabled{color:#c9c9c9}
</style>