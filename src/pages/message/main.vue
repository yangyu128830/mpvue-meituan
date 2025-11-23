<template>
  <div class="container">
    <div class="content">
      <!-- 消息列表 -->
      <div class="message-list">
        <!-- 示例消息，实际应从数据渲染 -->
        <div class="message-item" @click="viewMessageDetail">
          <div class="avatar"></div>
          <div class="message-content">
            <div class="sender">系统通知</div>
            <div class="time">2025-11-23 16:52</div>
            <div class="content">您有一张优惠券即将过期，请尽快使用！</div>
          </div>
        </div>
        <div class="message-item" @click="viewMessageDetail">
          <div class="avatar"></div>
          <div class="message-content">
            <div class="sender">订单通知</div>
            <div class="time">2025-11-23 15:34</div>
            <div class="content">您的订单已发货，请注意查收！</div>
          </div>
        </div>
        <div class="message-item" @click="viewMessageDetail">
          <div class="avatar"></div>
          <div class="message-content">
            <div class="sender">活动通知</div>
            <div class="time">2025-11-22 14:33</div>
            <div class="content">双十一促销活动即将开始，敬请期待！</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    viewMessageDetail() {
      // 跳转到消息详情页，这里需要根据实际路由调整
    }
  }
};
</script>

<style lang="scss">
.message-list {
  .message-item {
    display: flex;
    padding: 20rpx;
    background-color: white;
    margin-bottom: 10rpx;
    border-radius: 8rpx;
    box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.05);
    transition: all 0.3s ease;
    
    &:hover {
      transform: translateY(-2rpx);
      box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.1);
    }
    
    .avatar {
      width: 80rpx;
      height: 80rpx;
      border-radius: 50%;
      background-color: $theme-color;
      margin-right: 20rpx;
    }
    
    .message-content {
      flex: 1;
      
      .sender {
        font-size: 28rpx;
        color: $textBlack-color;
        font-weight: bold;
        margin-bottom: 10rpx;
      }
      
      .time {
        font-size: 20rpx;
        color: $textGray-color;
        margin-bottom: 10rpx;
      }
      
      .content {
        font-size: 24rpx;
        color: $textDarkGray-color;
        line-height: 1.5;
      }
    }
  }
}
</style>