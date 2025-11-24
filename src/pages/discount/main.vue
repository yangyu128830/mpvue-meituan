<template>
  <div class="discount-container">
    <!-- Header -->
    <div class="discount-header">
      <div class="header-title">特价</div>
      <div class="header-actions">
        <span class="search-btn" @click="searchClick">
          <i class="icon mt-search-o"></i>
        </span>
      </div>
    </div>

    <!-- Shopping cart items list -->
    <scroll-view class="discount-items" :scroll-y="true">
      <!-- Daily deals section -->
      <div class="daily-deals" v-if="dailyDeals.length > 0">
        <div class="section">
          <div class="l"></div>
          <span class="m">天天低价</span>
          <div class="r"></div>
        </div>
        <div class="deal-items">
          <div 
            class="deal-item" 
            v-for="(item, index) in dailyDeals"
            :key="index"
            @click="viewProductDetail(item)"
          >
            <div class="item-image">
              <img :src="item.picture || 'https://via.placeholder.com/100x100'" mode="aspectFill">
            </div>
            <div class="item-details">
              <div class="item-name">{{ item.name }}</div>
              <div class="item-price">¥{{ item.price }}</div>
            </div>
          </div>
        </div>
        <button class="view-all" @click="viewAllDeals">查看全部</button>
      </div>
      
      <!-- 分类标签 -->
      <div class="category-tabs">
        <ul>
          <li 
            v-for="(tab, index) in categoryTabs"
            :key="index"
            :class="{ active: activeTab === index }"
            @click="changeTab(index)"
          >
            {{ tab }}
          </li>
        </ul>
      </div>
      
      <!-- 商品列表 -->
      <div class="product-list">
        <div 
          class="product-item" 
          v-for="(item, index) in filteredProducts"
          :key="index"
          @click="viewProductDetail(item)"
        >
          <div class="item-image">
            <img :src="item.picture || 'https://via.placeholder.com/100x100'" mode="aspectFill">
          </div>
          <div class="item-details">
            <div class="item-name">{{ item.name }}</div>
            <div class="item-price">¥{{ item.price }}</div>
            <div class="item-desc" v-if="item.description">{{ item.description }}</div>
          </div>
        </div>
      </div>
      
      <!-- 我的爆料板块 -->
      <div class="my-exposure">
        <div class="section">
          <div class="l"></div>
          <span class="m">我的爆料</span>
          <div class="r"></div>
        </div>
        <button class="recommend-btn" @click="recommendProduct">推荐低价商品</button>
      </div>
    </scroll-view>
  </div>
</template>

<script>
export default {
  data() {
    return {
      activeTab: 0,
      categoryTabs: ['精选', '24h最热', '3h最热', '好价活动', '食品', '居家'],
      dailyDeals: [
        { id: 1, name: '特价商品1', price: 9.9, picture: 'https://via.placeholder.com/100x100' },
        { id: 2, name: '特价商品2', price: 19.9, picture: 'https://via.placeholder.com/100x100' },
        { id: 3, name: '特价商品3', price: 29.9, picture: 'https://via.placeholder.com/100x100' }
      ],
      products: [
        { id: 1, name: '精选商品1', price: 19.9, picture: 'https://via.placeholder.com/100x100', description: '精选商品描述' },
        { id: 2, name: '精选商品2', price: 29.9, picture: 'https://via.placeholder.com/100x100', description: '精选商品描述' },
        { id: 3, name: '精选商品3', price: 39.9, picture: 'https://via.placeholder.com/100x100', description: '精选商品描述' },
        { id: 4, name: '精选商品4', price: 49.9, picture: 'https://via.placeholder.com/100x100', description: '精选商品描述' },
        { id: 5, name: '精选商品5', price: 59.9, picture: 'https://via.placeholder.com/100x100', description: '精选商品描述' }
      ]
    };
  },
  computed: {
    filteredProducts() {
      // 根据当前标签过滤商品，实际应从API获取
      return this.products;
    }
  },
  methods: {
    searchClick() {
      wx.navigateTo({
        url: '/pages/searchList/main'
      });
    },
    changeTab(index) {
      this.activeTab = index;
      // 根据标签获取对应商品，实际应从API获取
    },
    viewProductDetail(item) {
      wx.navigateTo({
        url: '/pages/groupDetail/main'
      });
    },
    viewAllDeals() {
      wx.navigateTo({
        url: '/pages/allDeals/main'
      });
    },
    recommendProduct() {
      wx.navigateTo({
        url: '/pages/recommendProduct/main'
      });
    }
  }
};
</script>

<style lang="scss" scoped>
@import "@/assets/global.scss";

.discount-container {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  background-color: #f5f5f5;
}

/* Header styles */
.discount-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #ff6700;
  color: white;
  padding: 20rpx 30rpx;
  
  .header-title {
    font-size: 36rpx;
    font-weight: bold;
  }
  
  .search-btn {
    font-size: 28rpx;
    padding: 10rpx;
    border-radius: 50%;
    background-color: rgba(255, 255, 255, 0.2);
  }
}

/* Items list styles */
.discount-items {
  flex: 1;
  padding-bottom: 100rpx;
}

.daily-deals {
  background-color: white;
  margin: 10rpx;
  border-radius: 10rpx;
  padding: 20rpx;
  box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.05);
  
  .section {
    display: flex;
    align-items: center;
    justify-content: center;
    margin-bottom: 20rpx;
    
    .l, .r {
      height: 2rpx;
      width: 60rpx;
      background-color: #eee;
    }
    
    .m {
      font-size: 28rpx;
      color: #333;
      margin: 0 20rpx;
      font-weight: bold;
    }
  }
  
  .deal-items {
    display: flex;
    flex-direction: column;
    margin-bottom: 20rpx;
    
    .deal-item {
      display: flex;
      align-items: center;
      padding: 15rpx 0;
      border-bottom: 1px solid #eee;
      
      &:last-child {
        border-bottom: none;
      }
      
      .item-image {
        width: 120rpx;
        height: 120rpx;
        border-radius: 10rpx;
        overflow: hidden;
        margin-right: 20rpx;
        
        img {
          width: 100%;
          height: 100%;
          object-fit: cover;
        }
      }
      
      .item-details {
        flex: 1;
        
        .item-name {
          font-size: 30rpx;
          font-weight: bold;
          color: #333;
          margin-bottom: 10rpx;
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
        }
        
        .item-price {
          font-size: 28rpx;
          color: #ff6700;
          font-weight: bold;
        }
      }
    }
  }
      border-radius: 8rpx;
      padding: 10rpx;
      
      img {
        width: 100%;
        height: 120rpx;
        border-radius: 8rpx;
        margin-bottom: 10rpx;
      }
      
      .name-c {
        width: 100%;
        margin-bottom: 10rpx;
        
        .name {
          font-size: 20rpx;
          color: $textBlack-color;
          text-align: center;
          white-space: nowrap;
          overflow: hidden;
          text-overflow: ellipsis;
        }
      }
      
      .price {
        font-size: 24rpx;
        color: $mtRed-color;
        font-weight: bold;
      }
    }
  }
  
  .view-all {
    width: 100%;
    height: 60rpx;
    background-color: #ff6700;
    color: white;
    border: none;
    border-radius: 30rpx;
    font-size: 24rpx;
    font-weight: bold;
  }
}

.category-tabs {
  background-color: white;
  margin: 10rpx;
  border-radius: 10rpx;
  padding: 0 20rpx;
  box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.05);
  
  .tab-list {
    display: flex;
    flex-wrap: nowrap;
    overflow-x: auto;
    padding: 20rpx 0;
    
    .tab-item {
      font-size: 28rpx;
      color: #666;
      padding: 10rpx 20rpx;
      border-radius: 20rpx;
      margin-right: 15rpx;
      white-space: nowrap;
      
      &.active {
        background-color: #ff6700;
        color: white;
      }
    }
  }
}

.product-list {
  margin: 10rpx;
  background-color: white;
  border-radius: 10rpx;
  padding: 20rpx;
  box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.05);
  
  .product-item {
    display: flex;
    margin-bottom: 20rpx;
    padding-bottom: 20rpx;
    border-bottom: 1px solid #eee;
    
    &:last-child {
      border-bottom: none;
      margin-bottom: 0;
      padding-bottom: 0;
    }
    
    .item-image {
      width: 120rpx;
      height: 120rpx;
      border-radius: 10rpx;
      overflow: hidden;
      margin-right: 20rpx;
      
      img {
        width: 100%;
        height: 100%;
        object-fit: cover;
      }
    }
    
    .item-details {
      flex: 1;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      
      .item-name {
        font-size: 30rpx;
        color: #333;
        margin-bottom: 10rpx;
        overflow: hidden;
        text-overflow: ellipsis;
        display: -webkit-box;
        -webkit-line-clamp: 2;
        -webkit-box-orient: vertical;
      }
      
      .item-price {
        font-size: 28rpx;
        color: #ff6700;
        font-weight: bold;
      }
    }
  }
}

/* My exposure section styles */
.my-exposure {
  margin: 10rpx;
  background-color: white;
  border-radius: 10rpx;
  padding: 20rpx;
  box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.05);
  
  .section-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20rpx;
    
    .title {
      font-size: 30rpx;
      font-weight: bold;
      color: #333;
    }
    
    .more {
      font-size: 24rpx;
      color: #999;
    }
  }
  
  .exposure-items {
    display: flex;
    justify-content: space-between;
    
    .exposure-item {
      width: 32%;
      display: flex;
      flex-direction: column;
      align-items: center;
      background-color: #f5f5f5;
      border-radius: 10rpx;
      padding: 15rpx;
      
      .icon {
        font-size: 40rpx;
        color: #ff6700;
        margin-bottom: 10rpx;
      }
      
      .text {
        font-size: 24rpx;
        color: #333;
      }
    }
  }
  
  .recommend-btn {
    width: 100%;
    height: 60rpx;
    background-color: #ff6700;
    color: white;
    border: none;
    border-radius: 30rpx;
    font-size: 24rpx;
    font-weight: bold;
    margin-top: 20rpx;
  }
}
</style>