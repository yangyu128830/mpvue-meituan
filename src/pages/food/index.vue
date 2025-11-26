<template>
  <div class="food-page">
    <!-- Header Component -->
    <FoodHeader />
    
    <!-- Categories Component -->
    <FoodCategories @categoryChange="onCategoryChange" />
    
    <!-- Food List Component -->
    <FoodList :foodList="foodList" @addToCart="onAddToCart" @minusFromCart="onMinusFromCart" />
    
    <!-- Floating Cart Button -->
    <div class="floating-cart" @click="onCartClick">
      <i class="icon mt-cart-o"></i>
      <div class="cart-badge" v-if="cartTotal > 0">{{ cartTotal }}</div>
    </div>
  </div>
</template>

<script>
import FoodHeader from './components/FoodHeader.vue';
import FoodCategories from './components/FoodCategories.vue';
import FoodList from './components/FoodList.vue';

export default {
  name: 'FoodPage',
  components: {
    FoodHeader,
    FoodCategories,
    FoodList
  },
  data() {
    return {
      foodList: [
        {
          id: 1,
          name: '香辣鸡腿堡套餐',
          image: 'https://via.placeholder.com/180x180',
          tag: '热销',
          rating: 4.8,
          sales: 1234,
          distance: '1.2km',
          price: 29.9,
          originalPrice: 39.9,
          discount: '7.5折',
          tags: ['汉堡', '炸鸡', '套餐'],
          cartCount: 0
        },
        {
          id: 2,
          name: '麻辣香锅',
          image: 'https://via.placeholder.com/180x180',
          tag: '新品',
          rating: 4.7,
          sales: 890,
          distance: '800m',
          price: 35.8,
          originalPrice: 45.8,
          discount: '7.8折',
          tags: ['川菜', '麻辣', '香锅'],
          cartCount: 0
        },
        {
          id: 3,
          name: '日式拉面',
          image: 'https://via.placeholder.com/180x180',
          tag: '好评',
          rating: 4.9,
          sales: 1567,
          distance: '1.5km',
          price: 28.8,
          tags: ['日式', '拉面', '清淡'],
          cartCount: 0
        },
        {
          id: 4,
          name: '水果沙拉',
          image: 'https://via.placeholder.com/180x180',
          tag: '健康',
          rating: 4.6,
          sales: 678,
          distance: '1.0km',
          price: 18.8,
          tags: ['健康', '沙拉', '水果'],
          cartCount: 0
        }
      ],
      cartTotal: 0
    }
  },
  methods: {
    onCategoryChange(categoryIndex) {
      // Handle category change
      console.log('Category changed to:', categoryIndex);
    },
    onAddToCart(item) {
      item.cartCount++;
      this.cartTotal++;
      // Show success message
      wx.showToast({
        title: '已加入购物车',
        icon: 'success',
        duration: 1500
      });
    },
    onMinusFromCart(item) {
      if (item.cartCount > 0) {
        item.cartCount--;
        this.cartTotal--;
      }
    },
    onCartClick() {
      // Navigate to cart page
      wx.navigateTo({
        url: '/pages/shoppingCart/main'
      });
    }
  }
}
</script>

<style lang="scss" scoped>
.food-page {
  min-height: 100vh;
  background-color: #f5f5f5;
}

.floating-cart {
  position: fixed;
  bottom: 100rpx;
  right: 40rpx;
  width: 100rpx;
  height: 100rpx;
  border-radius: 50%;
  background-color: #ff4444;
  color: #fff;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.2);
  z-index: 999;
  
  i {
    font-size: 48rpx;
  }
  
  .cart-badge {
    position: absolute;
    top: -10rpx;
    right: -10rpx;
    width: 40rpx;
    height: 40rpx;
    border-radius: 50%;
    background-color: #fff;
    color: #ff4444;
    font-size: 24rpx;
    font-weight: bold;
    display: flex;
    align-items: center;
    justify-content: center;
  }
}
</style>