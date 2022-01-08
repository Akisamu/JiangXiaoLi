<template>
<uni-shadow-root class="field-index"><van-cell :icon="leftIcon" :title="label" :center="center" :border="border" :is-link="isLink" :required="required" :custom-style="customStyle" :title-width="titleWidth" custom-class="van-field">
  <slot name="label" slot="title"></slot>
  <view :class="'van-field__body '+(type === 'textarea' ? 'van-field__body--textarea' : '')">
    <textarea v-if="type === 'textarea'" :class="inputClass" :focus="focus" :value="value" :disabled="disabled || readonly" :maxlength="maxlength" :auto-height="autosize" :placeholder="placeholder" :placeholder-style="placeholderStyle" :placeholder-class="error ? 'van-field--error' : ''" :cursor-spacing="cursorSpacing" @input="onInput" @blur="onBlur" @focus="onFocus" @confirm="onConfirm"></textarea>
    <input v-else :class="inputClass" :type="type" :focus="focus" :value="value" :disabled="disabled || readonly" :maxlength="maxlength" :placeholder="placeholder" :placeholder-style="placeholderStyle" :placeholder-class="error ? 'van-field--error' : ''" :confirm-type="confirmType" :cursor-spacing="cursorSpacing" @input="onInput" @blur="onBlur" @focus="onFocus" @confirm="onConfirm"></input>
    <van-icon v-if="showClear" size="16px" name="clear" class="van-field__clear-root" custom-class="van-field__clear" @touchstart.native="onClear"></van-icon>
    <view class="van-field__icon-container" v-if="icon || useIconSlot" @click="onClickIcon">
      <van-icon v-if="icon" size="16px" :name="icon" :custom-class="'van-field__icon '+(iconClass)"></van-icon>
      <slot v-else name="icon"></slot>
    </view>
    <view v-if="useButtonSlot" class="van-field__button">
      <slot name="button"></slot>
    </view>
  </view>
  <view v-if="errorMessage" class="van-field__error-message">
    {{ errorMessage }}
  </view>
</van-cell></uni-shadow-root>
</template>

<script>
import VanCell from '../cell/index.vue'
import VanIcon from '../icon/index.vue'
global['__wxVueOptions'] = {components:{'van-cell': VanCell,'van-icon': VanIcon}}

global['__wxRoute'] = 'field/index'
import { VantComponent } from '../common/component';
VantComponent({
  field: true,
  classes: ['input-class'],
  props: {
    icon: String,
    label: String,
    error: Boolean,
    focus: Boolean,
    center: Boolean,
    isLink: Boolean,
    leftIcon: String,
    disabled: Boolean,
    autosize: Boolean,
    readonly: Boolean,
    required: Boolean,
    iconClass: String,
    clearable: Boolean,
    inputAlign: String,
    customClass: String,
    confirmType: String,
    errorMessage: String,
    placeholder: String,
    customStyle: String,
    useIconSlot: Boolean,
    useButtonSlot: Boolean,
    placeholderStyle: String,
    cursorSpacing: {
      type: Number,
      value: 50
    },
    maxlength: {
      type: Number,
      value: -1
    },
    type: {
      type: String,
      value: 'text'
    },
    border: {
      type: Boolean,
      value: true
    },
    titleWidth: {
      type: String,
      value: '90px'
    }
  },
  data: {
    showClear: false
  },
  computed: {
    inputClass: function inputClass() {
      var data = this.data;
      return this.classNames('input-class', 'van-field__input', {
        'van-field--error': data.error,
        'van-field__textarea': data.type === 'textarea',
        'van-field__input--disabled': data.disabled,
        ["van-field__input--" + data.inputAlign]: data.inputAlign
      });
    }
  },
  beforeCreate: function beforeCreate() {
    this.focused = false;
  },
  methods: {
    onInput: function onInput(event) {
      var _ref = event.detail || {},
          _ref$value = _ref.value,
          value = _ref$value === void 0 ? '' : _ref$value;

      this.$emit('input', value);
      this.$emit('change', value);
      this.setData({
        value: value,
        showClear: this.getShowClear(value)
      });
    },
    onFocus: function onFocus() {
      this.$emit('focus');
      this.focused = true;
      this.setData({
        showClear: this.getShowClear()
      });
    },
    onBlur: function onBlur() {
      this.focused = false;
      this.$emit('blur');
      this.setData({
        showClear: this.getShowClear()
      });
    },
    onClickIcon: function onClickIcon() {
      this.$emit('click-icon');
    },
    getShowClear: function getShowClear(value) {
      value = value === undefined ? this.data.value : value;
      return this.data.clearable && this.focused && value && !this.data.readonly;
    },
    onClear: function onClear() {
      this.setData({
        value: '',
        showClear: this.getShowClear('')
      });
      this.$emit('input', '');
      this.$emit('change', '');
    },
    onConfirm: function onConfirm() {
      this.$emit('confirm', this.data.value);
    }
  }
});
export default global['__wxComponents']['field/index']
</script>
<style platform="mp-weixin">
@import '../common/index.css';.van-field__body{display:-webkit-flex;display:flex;-webkit-align-items:center;align-items:center}.van-field__body--textarea{min-height:24px}.van-field__input{border:0;margin:0;padding:0;width:100%;height:24px;resize:none;display:block;text-align:left;min-height:24px;line-height:inherit;box-sizing:border-box;background-color:transparent}.van-field__input--disabled{opacity:1;color:#666;background-color:transparent}.van-field__input--center{text-align:center}.van-field__input--right{text-align:right}.van-field__clear-root{height:24px}.van-field__button,.van-field__clear,.van-field__icon-container{-webkit-flex-shrink:0;flex-shrink:0}.van-field__clear,.van-field__icon-container{padding:0 10px;line-height:inherit;margin-right:-10px;vertical-align:middle}.van-field__clear{color:#c9c9c9}.van-field__icon-container{color:#999}.van-field__icon{display:block!important}.van-field__button{padding-left:10px}.van-field__error-message{color:#f44;font-size:12px;text-align:left}.van-field--error{color:#f44}
</style>