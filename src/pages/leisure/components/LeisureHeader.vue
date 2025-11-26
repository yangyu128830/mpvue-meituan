<template>
  <div class="leisure-header">
    <div class="header-content">
      <div class="header-title">休闲玩乐</div>
      <div class="search-bar">
        <i class="icon mt-search-o"></i>
        <input placeholder="搜索休闲玩乐项目" placeholder-style="font-size: 24rpx" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'LeisureHeader'
};
</script>

<style lang="scss" scoped>
.leisure-header {
  background-color: white;
  padding: 16rpx 24rpx;
  box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.05);
  
  .header-content {
    display: flex;
    flex-direction: column;
    
    .header-title {
      font-size: 32rpx;
      font-weight: bold;
      color: #333;
      margin-bottom: 16rpx;
    }
    
    .search-bar {
      display: flex;
      align-items: center;
      background-color: #f5f5f5;
      border-radius: 40rpx;
      padding: 12rpx 20rpx;
      
      i {
        font-size: 24rpx;
        color: #999;
        margin-right: 12rpx;
      }
      
      input {
        flex: 1;
        font-size: 24rpx;
        color: #333;
        border: none;
        background-color: transparent;
      }
    }
  }
}
</style>