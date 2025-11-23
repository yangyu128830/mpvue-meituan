<template>
  <div class="container">
    <div class="search-history">
      <!-- Header -->
      <div class="history-header">
        <div class="header-left">
          <i class="icon mt-history" @click="goBack"></i>
          <span class="header-title">搜索历史</span>
        </div>
        <span class="clear-all" @click="clearHistory">清空历史</span>
      </div>
      
      <!-- Tabs -->
      <div class="history-tabs">
        <span class="tab" :class="{active: currentTab === 'product'}" @click="changeTab('product')">商品</span>
        <span class="tab" :class="{active: currentTab === 'store'}" @click="changeTab('store')">店铺</span>
        <span class="tab" :class="{active: currentTab === 'search'}" @click="changeTab('search')">搜索</span>
      </div>
      
      <!-- History List -->
      <div class="history-content" v-if="filteredHistory.length > 0">
        <div class="history-item" v-for="(item, index) in filteredHistory" :key="index">
          <div class="item-left" @click="onHistoryItemClick(item)">
            <i class="icon mt-history-item"></i>
            <span class="item-text">{{item.text}}</span>
          </div>
          <div class="item-right">
            <span class="item-time">{{item.time}}</span>
            <i class="icon mt-trash-o" @click.stop="deleteHistoryItem(index)"></i>
          </div>
        </div>
      </div>
      
      <!-- Empty State -->
      <div class="empty-state" v-else>
        <img src="https://via.placeholder.com/120" alt="No History" class="empty-image">
        <div class="empty-text">暂无搜索历史</div>
        <div class="empty-subtext">去搜索看看吧</div>
        <button class="empty-button" @click="goToSearch">去搜索</button>
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
  computed: {
    filteredHistory() {
      return this.historyList.filter(item => item.type === this.currentTab)
    }
  },
  mounted() {
    this.loadHistory()
  },
  methods: {
    loadHistory() {
      // For demo purposes, use sample data if no history exists
      let history = wx.getStorageSync('searchHistory')
      if (!history) {
        // Sample history data
        history = JSON.stringify([
          { text: '火锅', type: 'search', time: '今天 12:30' },
          { text: '海底捞', type: 'store', time: '今天 11:45' },
          { text: '麻辣香锅', type: 'search', time: '昨天 18:20' },
          { text: '北京烤鸭', type: 'product', time: '昨天 17:10' },
          { text: '奶茶', type: 'search', time: '前天 14:05' },
          { text: '星巴克', type: 'store', time: '前天 13:30' }
        ])
        wx.setStorageSync('searchHistory', history)
      }
      
      try {
        this.historyList = JSON.parse(history)
      } catch (e) {
        console.error('Failed to parse search history:', e)
      }
    },
    changeTab(tab) {
      this.currentTab = tab
    },
    onHistoryItemClick(item) {
      wx.navigateTo({
        url: `/pages/searchList/main?keyword=${encodeURIComponent(item.text)}`
      })
    },
    deleteHistoryItem(index) {
      this.historyList.splice(index, 1)
      wx.setStorageSync('searchHistory', JSON.stringify(this.historyList))
    },
    clearHistory() {
      wx.showModal({
        title: '确认清空',
        content: '是否要清空所有搜索历史？',
        confirmColor: '#FF6B00',
        success: (res) => {
          if (res.confirm) {
            this.historyList = []
            wx.removeStorageSync('searchHistory')
          }
        }
      })
    },
    goBack() {
      wx.navigateBack()
    },
    goToSearch() {
      wx.navigateTo({ url: '/pages/search/main' })
    }
  }
}
</script>

<style scoped>
.container {
  background-color: #f5f5f5;
  min-height: 100vh;
}

.search-history {
  background-color: #fff;
}

/* Header */
.history-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px 20px;
  border-bottom: 1px solid #eee;
}

.header-left {
  display: flex;
  align-items: center;
}

.header-left .icon {
  font-size: 20px;
  color: #333;
  margin-right: 16px;
}

.header-title {
  font-size: 18px;
  font-weight: 600;
  color: #333;
}

.clear-all {
  font-size: 14px;
  color: #999;
  padding: 6px 12px;
  border-radius: 20px;
  background-color: #f5f5f5;
}

.clear-all:active {
  background-color: #eee;
}

/* Tabs */
.history-tabs {
  display: flex;
  padding: 12px 20px;
  background-color: #fafafa;
}

.tab {
  flex: 1;
  text-align: center;
  padding: 8px 16px;
  font-size: 14px;
  color: #666;
  border-radius: 20px;
  margin-right: 10px;
}

.tab:last-child {
  margin-right: 0;
}

.tab.active {
  color: #fff;
  background-color: #FF6B00;
}

/* History List */
.history-content {
  padding: 8px 0;
}

.history-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px 20px;
  border-bottom: 1px solid #f5f5f5;
}

.history-item:last-child {
  border-bottom: none;
}

.item-left {
  display: flex;
  align-items: center;
  flex: 1;
}

.item-left .icon {
  font-size: 16px;
  color: #999;
  margin-right: 12px;
}

.item-text {
  font-size: 14px;
  color: #333;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.item-right {
  display: flex;
  align-items: center;
}

.item-time {
  font-size: 12px;
  color: #999;
  margin-right: 12px;
}

.item-right .icon {
  font-size: 16px;
  color: #999;
  padding: 4px;
  border-radius: 50%;
}

.item-right .icon:active {
  background-color: #f5f5f5;
}

/* Empty State */
.empty-state {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 60px 20px;
}

.empty-image {
  width: 120px;
  height: 120px;
  margin-bottom: 20px;
  opacity: 0.6;
}

.empty-text {
  font-size: 16px;
  color: #666;
  margin-bottom: 8px;
}

.empty-subtext {
  font-size: 14px;
  color: #999;
  margin-bottom: 32px;
}

.empty-button {
  padding: 10px 32px;
  border: none;
  border-radius: 24px;
  background-color: #FF6B00;
  color: #fff;
  font-size: 14px;
}

.empty-button:active {
  background-color: #E85F00;
}
</style>