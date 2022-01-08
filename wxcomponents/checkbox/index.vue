<template>
<uni-shadow-root class="checkbox-index"><view class="van-checkbox custom-class">
  <view class="van-checkbox__icon-wrap" @click="toggle">
    <slot v-if="useIconSlot" name="icon"></slot>
    <van-icon v-else name="success" :class="iconClass" :style="iconStyle" custom-class="icon-class" custom-style="line-height: 20px;"></van-icon>
  </view>
  <view :class="'van-checkbox__label van-checkbox__label--'+(labelPosition)+' label-class'" @click="onClickLabel">
    <slot></slot>
  </view>
</view></uni-shadow-root>
</template>

<script>
import VanIcon from '../icon/index.vue'
global['__wxVueOptions'] = {components:{'van-icon': VanIcon}}

global['__wxRoute'] = 'checkbox/index'
import { VantComponent } from '../common/component';
VantComponent({
  field: true,
  relation: {
    name: 'checkbox-group',
    type: 'ancestor'
  },
  classes: ['icon-class', 'label-class'],
  props: {
    value: null,
    disabled: Boolean,
    useIconSlot: Boolean,
    checkedColor: String,
    labelPosition: String,
    labelDisabled: Boolean,
    shape: {
      type: String,
      value: 'round'
    }
  },
  computed: {
    iconClass: function iconClass() {
      var _this$data = this.data,
          disabled = _this$data.disabled,
          value = _this$data.value,
          shape = _this$data.shape;
      return this.classNames('van-checkbox__icon', "van-checkbox__icon--" + shape, {
        'van-checkbox__icon--disabled': disabled,
        'van-checkbox__icon--checked': value
      });
    },
    iconStyle: function iconStyle() {
      var _this$data2 = this.data,
          value = _this$data2.value,
          disabled = _this$data2.disabled,
          checkedColor = _this$data2.checkedColor;

      if (checkedColor && value && !disabled) {
        return "border-color: " + checkedColor + "; background-color: " + checkedColor;
      }

      return '';
    }
  },
  methods: {
    emitChange: function emitChange(value) {
      var parent = this.getRelationNodes('../checkbox-group/index')[0];

      if (parent) {
        this.setParentValue(parent, value);
      } else {
        this.$emit('input', value);
        this.$emit('change', value);
      }
    },
    toggle: function toggle() {
      if (!this.data.disabled) {
        this.emitChange(!this.data.value);
      }
    },
    onClickLabel: function onClickLabel() {
      if (!this.data.disabled && !this.data.labelDisabled) {
        this.emitChange(!this.data.value);
      }
    },
    setParentValue: function setParentValue(parent, value) {
      var parentValue = parent.data.value.slice();
      var name = this.data.name;

      if (value) {
        if (parent.data.max && parentValue.length >= parent.data.max) {
          return;
        }
        /* istanbul ignore else */


        if (parentValue.indexOf(name) === -1) {
          parentValue.push(name);
          parent.$emit('input', parentValue);
          parent.$emit('change', parentValue);
        }
      } else {
        var index = parentValue.indexOf(name);
        /* istanbul ignore else */

        if (index !== -1) {
          parentValue.splice(index, 1);
          parent.$emit('input', parentValue);
          parent.$emit('change', parentValue);
        }
      }
    }
  }
});
export default global['__wxComponents']['checkbox/index']
</script>
<style platform="mp-weixin">
@import '../common/index.css';.van-checkbox{overflow:hidden;-webkit-user-select:none;user-select:none}.van-checkbox__icon-wrap,.van-checkbox__label{display:inline-block;line-height:20px;vertical-align:middle}.van-checkbox__icon{box-sizing:border-box;display:block;width:20px;height:20px;border:1px solid #e5e5e5;color:transparent;font-size:12px;text-align:center;transition:.2s}.van-checkbox__icon--round{border-radius:100%}.van-checkbox__icon--checked{color:#fff;border-color:#1989fa;background-color:#1989fa}.van-checkbox__icon--disabled{border-color:#eee;background-color:currentColor;color:#f8f8f8}.van-checkbox__icon--disabled.van-checkbox__icon--checked{border-color:#eee;background-color:#eee}.van-checkbox__label{margin-left:10px}.van-checkbox__label--left{margin:0 10px 0 0;float:left}.van-checkbox__label:empty{margin:0}
</style>