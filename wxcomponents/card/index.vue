<template>
<uni-shadow-root class="card-index"><view :class="'custom-class van-card '+(centered ? 'van-card--center' : '')">
  <view class="van-card__thumb" @click="onClickThumb">
    <image v-if="thumb" :src="thumb" :mode="thumbMode" :lazy-load="lazyLoad" class="van-card__img thumb-class"></image>
    <slot v-else name="thumb"></slot>
    <van-tag v-if="tag" mark type="danger" custom-class="van-card__tag">
      {{ tag }}
    </van-tag>
  </view>

  <view class="van-card__content">
    <view class="van-card__left">
      <view v-if="title" class="van-card__title van-multi-ellipsis--l2 title-class">{{ title }}</view>
      <slot v-else name="title"></slot>

      <view v-if="desc" class="van-card__desc van-ellipsis desc-class">{{ desc }}</view>
      <slot v-else name="desc"></slot>

      <slot name="tags"></slot>
    </view>
    <view class="van-card__right">
      <view v-if="price || price === 0" class="van-card__price price-class">{{ currency }} {{ price }}</view>
      <view v-if="originPrice || originPrice === 0" class="van-card__origin-price origin-price-class">{{ currency }} {{ originPrice }}</view>
      <view v-if="num" class="van-card__num num-class">x {{ num }}</view>
    </view>
  </view>

  <view class="van-card__footer">
    <slot name="footer"></slot>
  </view>
</view></uni-shadow-root>
</template>

<script>
import VanTag from '../tag/index.vue'
global['__wxVueOptions'] = {components:{'van-tag': VanTag}}

global['__wxRoute'] = 'card/index'
import { link } from '../mixins/link';
import { VantComponent } from '../common/component';
VantComponent({
  classes: ['num-class', 'desc-class', 'thumb-class', 'title-class', 'price-class', 'origin-price-class'],
  mixins: [link],
  props: {
    tag: String,
    num: String,
    desc: String,
    thumb: String,
    title: String,
    price: String,
    centered: Boolean,
    lazyLoad: Boolean,
    thumbLink: String,
    originPrice: String,
    thumbMode: {
      type: String,
      value: 'scaleToFill'
    },
    currency: {
      type: String,
      value: '¥'
    }
  },
  methods: {
    onClickThumb: function onClickThumb() {
      this.jumpLink('thumbLink');
    }
  }
});
export default global['__wxComponents']['card/index']
</script>
<style platform="mp-weixin">
@import '../common/index.css';.van-card{box-sizing:border-box;position:relative;height:100px;font-size:12px;color:#333;padding:5px 15px 5px 115px;background:#fafafa}.van-card--center{-webkit-align-items:center;align-items:center;-webkit-justify-content:center;justify-content:center}.van-card__thumb{position:absolute;top:5px;left:15px;width:90px;height:90px}.van-card__img{width:100%;height:100%}.van-card,.van-card__content{display:-webkit-flex;display:flex}.van-card__content{width:100%}.van-card__desc,.van-card__title{line-height:20px;word-break:break-all}.van-card__title{max-height:40px}.van-card__desc{max-height:20px;color:#666}.van-card__left{-webkit-flex:1;flex:1;min-width:0}.van-card__right{-webkit-flex:none;flex:none;padding-left:10px;line-height:20px;text-align:right}.van-card__origin-price{color:#666;text-decoration:line-through}.van-card__num{color:#666}.van-card__tag{position:absolute;top:2px;left:0}.van-card__footer{position:absolute;right:15px;bottom:5px}.van-card__footer .van-button{margin-left:5px}
</style>