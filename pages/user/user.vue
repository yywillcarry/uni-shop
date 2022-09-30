<template>
  <view class="my-container">
    <login v-if="!token"></login>
    <userinfo v-else></userinfo>
  </view>
</template>

<script>
 import badgeMix from '@/mixins/tabbar-badge.js'
 import {mapState} from 'vuex'
  export default {
    mixins:[badgeMix],
    data() {
      return {
        
      };
    },
      computed: {
        // 2. 从 m_user 模块中导入需要的 token 字符串
        ...mapState('user', ['token']),
      },
  }
</script>

<style lang="scss">
page,
.my-container {
  height: 100%;
}
</style>
