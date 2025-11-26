<template>
  <div class="food-list">
    <div class="food-item" v-for="(item, index) in foodList" :key="index"
         @click="onFoodItemClick(item)">
      <div class="item-image">
        <img :src="item.image" alt="Food Image">
        <div class="tag" v-if="item.tag">{{ item.tag }}</div>
      </div>
      <div class="item-info">
        <div class="item-name">{{ item.name }}</div>
        <div class="item-rating">
          <i class="icon mt-star-s" v-for="star in Math.floor(item.rating)" :key="star"></i>
          <i class="icon mt-star-half-o" v-if="item.rating % 1 !== 0"></i>
          <span>{{ item.rating }}</span>
          <span class="sales">月售{{ item.sales }}</span>
          <span class="distance">{{ item.distance }}</span>
        </div>
        <div class="item-price">
          <span class="current-price">¥{{ item.price }}</span>
          <span class="original-price" v-if="item.originalPrice">¥{{ item.originalPrice }}</span>
          <span class="discount" v-if="item.discount">{{ item.discount }}</span>
        </div>
        <div class="item-tags">
          <span class="tag" v-for="(tag, idx) in item.tags" :key="idx">{{ tag }}</span>
        </div>
      </div>
      <div class="item-actions">
        <button class="add-btn" @click.stop="onAddToCart(item)">
          <i class="icon mt-plus-o"></i>
        </button>
        <div class="cart-count" v-if="item.cartCount > 0">
          <button class="minus-btn" @click.stop="onMinusFromCart(item)">
            <i class="icon mt-minus-o"></i>
          </button>
          <span class="count">{{ item.cartCount }}</span>
          <button class="plus-btn" @click.stop="onAddToCart(item)">
            <i class="icon mt-plus-o"></i>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'FoodList',
  props: {
    foodList: {
      type: Array,
      default: () => []
    }
  },
  methods: {
    onFoodItemClick(item) {
      // Navigate to food detail page
      wx.navigateTo({
        url: `/pages/food/detail?foodId=${item.id}`
      });
    },
    onAddToCart(item) {
      // Add to cart logic
      this.$emit('addToCart', item);
    },
    onMinusFromCart(item) {
      // Minus from cart logic
      this.$emit('minusFromCart', item);
    }
  }
}
</script>

<style lang="scss" scoped>
.food-list {
  background-color: #f5f5f5;
  padding: 10rpx;
  
  .food-item {
    display: flex;
    background-color: #fff;
    border-radius: 10rpx;
    padding: 20rpx;
    margin-bottom: 10rpx;
    
    .item-image {
      width: 180rpx;
      height: 180rpx;
      border-radius: 10rpx;
      overflow: hidden;
      position: relative;
      
      img {
        width: 100%;
        height: 100%;
        object-fit: cover;
      }
      
      .tag {
        position: absolute;
        top: 10rpx;
        left: 10rpx;
        background-color: #ff4444;
        color: #fff;
        font-size: 20rpx;
        padding: 4rpx 8rpx;
        border-radius: 4rpx;
      }
    }
    
    .item-info {
      flex: 1;
      margin-left: 20rpx;
      
      .item-name {
        font-size: 30rpx;
        color: #333;
        margin-bottom: 10rpx;
        line-height: 1.4;
        overflow: hidden;
        text-overflow: ellipsis;
        display: -webkit-box;
        -webkit-line-clamp: 2;
        -webkit-box-orient: vertical;
      }
      
      .item-rating {
        font-size: 24rpx;
        color: #999;
        margin-bottom: 10rpx;
        
        i {
          color: #ffcc00;
          margin-right: 4rpx;
        }
        
        span {
          margin-right: 10rpx;
        }
        
        .sales, .distance {
          color: #999;
        }
      }
      
      .item-price {
        font-size: 28rpx;
        margin-bottom: 10rpx;
        
        .current-price {
          color: #ff4444;
          font-weight: bold;
          margin-right: 10rpx;
        }
        
        .original-price {
          color: #999;
          text-decoration: line-through;
          margin-right: 10rpx;
        }
        
        .discount {
          color: #ff4444;
        }
      }
      
      .item-tags {
        
        .tag {
          display: inline-block;
          font-size: 22rpx;
          color: #666;
          background-color: #f5f5f5;
          padding: 4rpx 8rpx;
          border-radius: 4rpx;
          margin-right: 8rpx;
          margin-bottom: 8rpx;
        }
      }
    }
    
    .item-actions {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      
      .add-btn {
        width: 60rpx;
        height: 60rpx;
        border-radius: 50%;
        background-color: #ff4444;
        color: #fff;
        border: none;
        outline: none;
        display: flex;
        align-items: center;
        justify-content: center;
      }
      
      .cart-count {
        display: flex;
        align-items: center;
        
        button {
          width: 40rpx;
          height: 40rpx;
          border-radius: 50%;
          background-color: #fff;
          color: #ff4444;
          border: 1px solid #ff4444;
          outline: none;
          display: flex;
          align-items: center;
          justify-content: center;
        }
        
        .count {
          margin: 0 10rpx;
          font-size: 24rpx;
          color: #333;
        }
      }
    }
  }
}
</style>