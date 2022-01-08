<template>
<uni-shadow-root class="tab-index"><view v-if="inited" class="custom-class van-tab__pane" :style="active ? '' : 'display: none'">
  <slot></slot>
</view></uni-shadow-root>
</template>

<script>

global['__wxRoute'] = 'tab/index'
import { VantComponent } from '../common/component';
VantComponent({
  relation: {
    name: 'tabs',
    type: 'ancestor'
  },
  props: {
    title: String,
    disabled: Boolean
  },
  data: {
    inited: false,
    active: false
  },
  watch: {
    disabled: function disabled() {
      var parent = this.getRelationNodes('../tabs/index')[0];

      if (parent) {
        parent.updateTabs();
      }
    },
    title: function title() {
      var parent = this.getRelationNodes('../tabs/index')[0];

      if (parent) {
        parent.updateTabs();
      }
    }
  }
});
export default global['__wxComponents']['tab/index']
</script>
<style platform="mp-weixin">

</style>