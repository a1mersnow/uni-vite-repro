<template>
  <view class="content">
    <image class="logo" src="/static/logo.png"></image>
    <view class="text-area">
      <text class="title">{{ title }}</text>
    </view>
    <view>
      <button @click="handleGoTo">goto other</button>
    </view>
  </view>
</template>

<script>
import logo from '../../static/logo.png'
console.log(logo)

export default {
  data() {
    return {
      title: 'Hello',
    }
  },
  onLoad() {},
  methods: {
    handleGoTo () {
      uni.navigateTo({
        url: '/pagesOther/other/other'
      })
    }
  },
}
</script>

<style>
page {
  background-color: blue;
}
</style>

<style scoped>
.content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.logo {
  height: 200rpx;
  width: 200rpx;
  margin-top: 200rpx;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 50rpx;
}

.text-area {
  display: flex;
  justify-content: center;
}

.title {
  font-size: 36rpx;
  color: #8f8f94;
}
</style>
