<template>
  <div class="container">
    <div class="header-c">
      <button @click="goBack">返回</button>
      <h2>会员等级</h2>
    </div>
    <div class="content-c">
      <div class="level-card" :class="currentLevelClass">
        <div class="level-icon">{{ currentLevelIcon }}</div>
        <div class="level-name">{{ currentLevelName }}</div>
        <div class="level-desc">当前消费：{{ totalConsumption }} 元</div>
        <div class="progress" v-if="currentLevel !== '黑金会员'">
          <div class="progress-bar" :style="{ width: progressWidth + '%' }"></div>
          <div class="progress-text">{{ currentConsumption }}/{{ nextLevelRequirement }} 元</div>
        </div>
      </div>
      
      <div class="level-list">
        <div class="level-item" v-for="(item, index) in levelList" :key="index">
          <div class="level-icon">{{ item.icon }}</div>
          <div class="level-info">
            <div class="level-name">{{ item.name }}</div>
            <div class="level-requirement">需消费 {{ item.requirement }} 元</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      totalConsumption: 0,
      levelList: [
        { name: '普通会员', requirement: 0, icon: '★', class: 'normal' },
        { name: '白银会员', requirement: 500, icon: '★★', class: 'silver' },
        { name: '黄金会员', requirement: 1000, icon: '★★★', class: 'gold' },
        { name: '黑金会员', requirement: 10000, icon: '★★★★', class: 'black-gold' }
      ]
    }
  },
  computed: {
    currentLevel() {
      if (this.totalConsumption >= 10000) return '黑金会员'
      if (this.totalConsumption >= 1000) return '黄金会员'
      if (this.totalConsumption >= 500) return '白银会员'
      return '普通会员'
    },
    currentLevelName() {
      return this.currentLevel
    },
    currentLevelIcon() {
      const level = this.currentLevel
      if (level === '普通会员') return '★'
      if (level === '白银会员') return '★★'
      if (level === '黄金会员') return '★★★'
      if (level === '黑金会员') return '★★★★'
    },
    currentLevelClass() {
      const level = this.currentLevel
      if (level === '普通会员') return 'normal'
      if (level === '白银会员') return 'silver'
      if (level === '黄金会员') return 'gold'
      if (level === '黑金会员') return 'black-gold'
    },
    currentConsumption() {
      if (this.totalConsumption >= 10000) return 10000
      if (this.totalConsumption >= 1000) return this.totalConsumption - 1000
      if (this.totalConsumption >= 500) return this.totalConsumption - 500
      return this.totalConsumption
    },
    nextLevelRequirement() {
      if (this.totalConsumption >= 10000) return 10000
      if (this.totalConsumption >= 1000) return 10000 - 1000
      if (this.totalConsumption >= 500) return 1000 - 500
      return 500
    },
    progressWidth() {
      if (this.totalConsumption >= 10000) return 100
      return (this.currentConsumption / this.nextLevelRequirement) * 100
    }
  },
  methods: {
    goBack() {
      wx.navigateBack()
    }
  },
  onLoad() {
    // Get total consumption from local storage
    const orders = wx.getStorageSync('orders') || []
    this.totalConsumption = orders.reduce((sum, order) => sum + order.amount, 0)
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
  }
  
  .content-c {
    padding: 40rpx 20rpx;
    
    .level-card {
      background-color: white;
      border-radius: 10rpx;
      padding: 40rpx;
      margin-bottom: 40rpx;
      text-align: center;
      
      &.normal {
        background-color: #f5f5f5;
      }
      
      &.silver {
        background-color: #e0e0e0;
      }
      
      &.gold {
        background-color: #FFD26B;
      }
      
      &.black-gold {
        background-color: #333;
        color: white;
      }
      
      .level-icon {
        font-size: 60rpx;
        margin-bottom: 20rpx;
      }
      
      .level-name {
        font-size: 36rpx;
        font-weight: bold;
        margin-bottom: 10rpx;
      }
      
      .level-desc {
        font-size: 28rpx;
        margin-bottom: 20rpx;
      }
      
      .progress {
        height: 20rpx;
        background-color: rgba(0, 0, 0, 0.1);
        border-radius: 10rpx;
        margin-bottom: 10rpx;
        overflow: hidden;
        
        .progress-bar {
          height: 100%;
          background-color: #FF6B00;
        }
      }
      
      .progress-text {
        font-size: 24rpx;
        color: #666;
      }
    }
    
    .level-list {
      background-color: white;
      border-radius: 10rpx;
      
      .level-item {
        display: flex;
        align-items: center;
        padding: 20rpx 40rpx;
        border-bottom: 2rpx solid #f0f0f0;
        
        &:last-child {
          border-bottom: none;
        }
        
        .level-icon {
          font-size: 40rpx;
          margin-right: 20rpx;
        }
        
        .level-info {
          .level-name {
            font-size: 28rpx;
            font-weight: bold;
            margin-bottom: 5rpx;
          }
          
          .level-requirement {
            font-size: 24rpx;
            color: #666;
          }
        }
      }
    }
  }
}
</style>