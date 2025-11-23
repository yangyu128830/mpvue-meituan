<template>
  <div class="container">
    <div class="header-c">
      <button @click="goBack">返回</button>
      <h2>最近访问</h2>
      <button @click="clearHistory">清空历史</button>
    </div>
    <div class="store-list" v-if="visitedStores.length > 0">
      <div class="store-item" v-for="(store, index) in visitedStores" :key="index" @click="visitStore(store)">
        <img :src="store.image" alt="" class="store-image">
        <div class="store-info">
          <div class="store-name">{{ store.name }}</div>
          <div class="store-desc">{{ store.desc }}</div>
          <div class="store-meta">
            <span class="store-rating">{{ store.rating }}</span>
            <span class="store-delivery-time">{{ store.deliveryTime }}分钟</span>
            <span class="store-distance">{{ store.distance }}km</span>
          </div>
        </div>
      </div>
    </div>
    <div class="empty-state" v-else>
      <img src="https://cdn.pixabay.com/photo/2016/11/01/14/58/nurse-1789400_960_720.png" alt="无历史记录">
      <p>暂无访问记录</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      visitedStores: []
    }
  },
  methods: {
    goBack() {
      wx.navigateBack()
    },
    clearHistory() {
      wx.showModal({
        title: '确认清空？',
        content: '清空后将无法恢复历史记录',
        success: (res) => {
          if (res.confirm) {
            wx.removeStorageSync('visitedStores')
            this.visitedStores = []
          }
        }
      })
    },
    visitStore(store) {
      // Update visit time
      store.visitTime = new Date().toISOString()
      // Save to local storage
      this.updateVisitedStores(store)
      // Navigate to store detail page (demo)
      wx.showToast({
        title: `正在访问${store.name}`,
        icon: 'none'
      })
    },
    updateVisitedStores(store) {
      // Get current visited stores
      let visitedStores = wx.getStorageSync('visitedStores') || []
      // Remove existing entry if any
      visitedStores = visitedStores.filter(s => s.id !== store.id)
      // Add to the beginning
      visitedStores.unshift(store)
      // Keep only last 10
      if (visitedStores.length > 10) {
        visitedStores = visitedStores.slice(0, 10)
      }
      // Save back to local storage
      wx.setStorageSync('visitedStores', visitedStores)
      this.visitedStores = visitedStores
    }
  },
  onLoad() {
    // Get visited stores from local storage, or generate sample data if none
    let visitedStores = wx.getStorageSync('visitedStores')
    if (!visitedStores || visitedStores.length === 0) {
      // Generate sample data
      visitedStores = [
        {
          id: 1,
          name: '麦当劳(中关村店)',
          desc: '经典汉堡，全球连锁',
          rating: '4.8',
          deliveryTime: 30,
          distance: 1.2,
          image: 'https://cdn.pixabay.com/photo/2016/11/01/14/58/nurse-1789400_960_720.png',
          visitTime: new Date().toISOString()
        },
        {
          id: 2,
          name: '肯德基(五道口店)',
          desc: '炸鸡专家，全家桶',
          rating: '4.7',
          deliveryTime: 25,
          distance: 1.5,
          image: 'https://cdn.pixabay.com/photo/2016/11/01/14/58/nurse-1789400_960_720.png',
          visitTime: new Date(new Date().getTime() - 3600000).toISOString()
        },
        {
          id: 3,
          name: '必胜客(国贸店)',
          desc: '披萨意面，欢乐分享',
          rating: '4.9',
          deliveryTime: 35,
          distance: 2.0,
          image: 'https://cdn.pixabay.com/photo/2016/11/01/14/58/nurse-1789400_960_720.png',
          visitTime: new Date(new Date().getTime() - 7200000).toISOString()
        }
      ]
      // Save sample data to local storage
      wx.setStorageSync('visitedStores', visitedStores)
    }
    this.visitedStores = visitedStores
  }
}
</script>

<style lang="scss" scoped>
.container {
  .header-c {
    display: flex;
    align-items: center;
    height: 80rpx;
    background-color: white;
    padding: 0 20rpx;
    border-bottom: 2rpx solid #f0f0f0;
    
    button {
      background: none;
      border: none;
      font-size: 28rpx;
      color: #333;
    }
    
    h2 {
      flex: 1;
      text-align: center;
      font-size: 32rpx;
      color: #333;
      margin: 0;
    }
    
    button:last-child {
      color: #FF6B00;
    }
  }
  
  .store-list {
    background-color: white;
    margin-top: 20rpx;
    
    .store-item {
      display: flex;
      align-items: center;
      padding: 20rpx;
      border-bottom: 2rpx solid #f0f0f0;
      cursor: pointer;
      
      &:last-child {
        border-bottom: none;
      }
      
      .store-image {
        width: 120rpx;
        height: 120rpx;
        border-radius: 10rpx;
        margin-right: 20rpx;
      }
      
      .store-info {
        flex: 1;
        
        .store-name {
          font-size: 28rpx;
          color: #333;
          margin-bottom: 10rpx;
          font-weight: bold;
        }
        
        .store-desc {
          font-size: 24rpx;
          color: #666;
          margin-bottom: 10rpx;
        }
        
        .store-meta {
          display: flex;
          font-size: 22rpx;
          color: #999;
          
          span {
            margin-right: 20rpx;
          }
        }
      }
    }
  }
  
  .empty-state {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 60vh;
    
    img {
      width: 200rpx;
      height: 200rpx;
      margin-bottom: 20rpx;
    }
    
    p {
      font-size: 28rpx;
      color: #666;
    }
  }
}
</style>