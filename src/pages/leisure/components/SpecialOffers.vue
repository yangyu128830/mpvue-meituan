<template>
  <div class="special-offers">
    <div class="section-header">
      <div class="section-title">
        <i class="icon mt-fire-o"></i>
        <span>特价团限时秒杀</span>
      </div>
      <div class="time-countdown">3小时内有效</div>
    </div>
    
    <div class="offers-list">
      <div class="offer-item" v-for="(offer, index) in specialOffers" :key="index">
        <div class="offer-image">
          <img :src="offer.image" alt="Offer image" />
        </div>
        <div class="offer-details">
          <div class="offer-name">{{ offer.name }}</div>
          <div class="price-info">
            <span class="original-price">¥{{ offer.originalPrice }}</span>
            <span class="discount-price">¥{{ offer.discountPrice }}</span>
          </div>
          <div class="time-left">剩余时间：{{ offer.timeLeft }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SpecialOffers',
  props: {
    specialOffers: {
      type: Array,
      required: true
    }
  }
};
</script>

<style lang="scss" scoped>
.special-offers {
  background-color: white;
  margin-bottom: 16rpx;
  
  .section-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 16rpx 24rpx;
    border-bottom: 1rpx solid #f5f5f5;
    
    .section-title {
      display: flex;
      align-items: center;
      font-size: 28rpx;
      color: #333;
      font-weight: bold;
      
      i {
        color: #ff6700;
        margin-right: 8rpx;
      }
    }
    
    .time-countdown {
      font-size: 24rpx;
      color: #ff6700;
    }
  }
  
  .offers-list {
    display: flex;
    overflow-x: auto;
    padding: 16rpx;
    
    .offer-item {
      flex: 0 0 auto;
      width: 280rpx;
      margin-right: 16rpx;
      
      .offer-image {
        width: 100%;
        height: 200rpx;
        margin-bottom: 8rpx;
        
        img {
          width: 100%;
          height: 100%;
          border-radius: 8rpx;
          object-fit: cover;
        }
      }
      
      .offer-details {
        .offer-name {
          font-size: 24rpx;
          color: #333;
          margin-bottom: 8rpx;
          line-height: 1.4;
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
        }
        
        .price-info {
          display: flex;
          align-items: baseline;
          margin-bottom: 8rpx;
          
          .original-price {
            font-size: 20rpx;
            color: #999;
            text-decoration: line-through;
            margin-right: 8rpx;
          }
          
          .discount-price {
            font-size: 28rpx;
            color: #ff6700;
          }
        }
        
        .time-left {
          font-size: 20rpx;
          color: #666;
        }
      }
    }
  }
}
</style>