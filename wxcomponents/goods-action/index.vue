<template>
<uni-shadow-root class="goods-action-index"><view class="van-goods-action custom-class">
  <slot></slot>
</view></uni-shadow-root>
</template>

<script>

global['__wxRoute'] = 'goods-action/index'
import { VantComponent } from '../common/component';
VantComponent();
export default global['__wxComponents']['goods-action/index']
</script>
<style platform="mp-weixin">
@import '../common/index.css';.van-goods-action{left:0;right:0;bottom:0;display:-webkit-flex;display:flex;position:fixed}
</style>