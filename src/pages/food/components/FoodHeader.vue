<template>
  <div class="food-header">
    <!-- Location Selector -->
    <div class="location" @click="onLocationClick">
      <i class="icon mt-location-o"></i>
      <span>{{ currentLocation }}</span>
      <i class="icon mt-arrow-right-o"></i>
    </div>
    
    <!-- Search Bar -->
    <div class="search-bar" @click="onSearchClick">
      <i class="icon mt-search-o"></i>
      <input type="text" placeholder="搜索美食、商家或商品" v-model="searchKeyword" readonly>
    </div>
    
    <!-- Scan Icon -->
    <div class="scan-icon" @click="onScanClick">
      <i class="icon mt-scan-o"></i>
    </div>
  </div>
</template>

<script>
export default {
  name: 'FoodHeader',
  data() {
    return {
      currentLocation: '上海市漕河泾开发区',
      searchKeyword: ''
    }
  },
  methods: {
    onLocationClick() {
      // Handle location selection
    },
    onSearchClick() {
      // Handle search
    },
    onScanClick() {
      // Handle scan
    }
  }
}
</script>

<style lang="scss" scoped>
.food-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 20rpx;
  background-color: #fff;
  border-bottom: 1px solid #f0f0f0;
  
  .location {
    display: flex;
    align-items: center;
    color: #333;
    font-size: 28rpx;
    
    i {
      margin-right: 10rpx;
      font-size: 32rpx;
    }
    
    i:last-child {
      margin-left: 10rpx;
      font-size: 24rpx;
    }
  }
  
  .search-bar {
    flex: 1;
    display: flex;
    align-items: center;
    background-color: #f5f5f5;
    border-radius: 40rpx;
    padding: 12rpx 20rpx;
    margin: 0 20rpx;
    
    i {
      color: #999;
      font-size: 28rpx;
      margin-right: 10rpx;
    }
    
    input {
      flex: 1;
      background-color: transparent;
      border: none;
      outline: none;
      font-size: 28rpx;
      color: #333;
    }
  }
  
  .scan-icon {
    width: 60rpx;
    height: 60rpx;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #ff4444;
    border-radius: 50%;
    
    i {
      color: #fff;
      font-size: 32rpx;
    }
  }
}
</style>