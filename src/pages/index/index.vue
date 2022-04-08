<template>
  <view class="content">
    <image class="logo" src="/static/logo.png"></image>
    <view class="text-area">
      <text class="title">{{ title }}</text>
    </view>
    <view>
      <button @click="handleGoTo">goto other</button>
    </view>

    <view>
      <button @click="showPopup">showPop</button>
    </view>

    outer:{{count}}

    <popup-local
      mask-click
      type="bottom"
      ref="popup"
    >
      <view style="height: 300px; background-color: orange;">
        <view>count: {{count}}</view>
        <button @click="count++">add</button>
      </view>
    </popup-local>
  </view>
</template>

<script>
import logo from '../../static/logo.png'
import PopupLocal from '../../components/popup-local/popup-local.vue'
console.log(logo)

export default {
  components: {
    PopupLocal,
  },
  data() {
    return {
      title: 'Hello',
      count: 0,
    }
  },
  onLoad() {},
  methods: {
    handleGoTo () {
      uni.navigateTo({
        url: '/pagesOther/other/other'
      })
    },
    showPopup () {
      this.$refs.popup.open()
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
