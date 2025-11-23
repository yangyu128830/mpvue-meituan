<template>
  <div class="container">
    <div class="content">
      <div class="header">
        <span class="title">浏览记录</span>
        <span class="clear" @click="clearHistory">清空历史</span>
      </div>
      <div class="tabs">
        <span class="tab" :class="{active: currentTab === 'product'}" @click="currentTab = 'product'">商品</span>
        <span class="tab" :class="{active: currentTab === 'store'}" @click="currentTab = 'store'">店铺</span>
        <span class="tab" :class="{active: currentTab === 'search'}" @click="currentTab = 'search'">搜索</span>
      </div>
      <div class="line"></div>
      <div class="history-list" v-if="historyList.length > 0">
        <div class="item" v-for="(item, index) in historyList" :key="index" @click="onHistoryItemClick(item)">
          <span>{{item}}</span>
          <i class="icon mt-trash-o" @click.stop="deleteHistoryItem(index)"></i>
        </div>
      </div>
      <div class="empty" v-else>
        <span>暂无浏览记录</span>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  data() {
    return {
      historyList: [],
      currentTab: 'search'
    }
  },
  mounted() {
    this.loadHistory()
  },
  methods: {
    loadHistory() {
      const history = wx.getStorageSync('browseHistory')
      if (history) {
        try {
          this.historyList = JSON.parse(history)
        } catch (e) {
          console.error('Failed to parse browse history:', e)
        }
      }
    },
    onHistoryItemClick(item) {
      wx.navigateTo({
        url: `/pages/searchList/main?keyword=${encodeURIComponent(item)}`
      })
    },
    deleteHistoryItem(index) {
      this.historyList.splice(index, 1)
      wx.setStorageSync('browseHistory', JSON.stringify(this.historyList))
    },
    clearHistory() {
      this.historyList = []
      wx.removeStorageSync('browseHistory')
    }
  }
}
</script>

<style lang="scss" scoped>
.container {
  .content {
    background-color: white;
    display: flex;
    flex-direction: column;
    .header {
      display: flex;
      align-items: center;
      padding: 30rpx;
      .title {
        flex: 1;
        font-size: 32rpx;
        color: $textBlack-color;
      }
      .clear {
        font-size: 24rpx;
        color: $textDarkGray-color;
      }
    }
    .tabs {
      display: flex;
      justify-content: space-around;
      padding: 20rpx 0;
      .tab {
        font-size: 28rpx;
        color: $textDarkGray-color;
        padding: 10rpx;
        border-bottom: 2rpx solid transparent;
        &.active {
          color: $theme-color;
          border-bottom-color: $theme-color;
        }
      }
    }
    .line {
      height: 2rpx;
      background-color: $spLine-color;
    }
    .history-list {
      .item {
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 30rpx;
        border-bottom: 1rpx solid $spLine-color;
        span {
          font-size: 28rpx;
          color: $textBlack-color;
        }
        i {
          font-size: 32rpx;
          color: $textDarkGray-color;
        }
      }
    }
    .empty {
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 100rpx;
      span {
        font-size: 28rpx;
        color: $textDarkGray-color;
      }
    }
  }
}
</style>