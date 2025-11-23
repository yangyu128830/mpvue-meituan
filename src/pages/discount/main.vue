<template>
  <div class="container">
    <div class="content">
      <!-- 顶部搜索框 -->
      <div class="search-box">
        <i class="icon mt-search-o"></i>
        <input type="text" placeholder="搜索商品">
      </div>
      
      <!-- 天天低价商品框 -->
      <div class="daily-deals">
        <div class="section">
          <div class="l"></div>
          <span class="m">天天低价</span>
          <div class="r"></div>
        </div>
        <div class="deal-items">
          <!-- 示例商品，实际应从数据渲染 -->
          <div class="deal-item" @click="viewProductDetail">
            <img src="/static/images/icon_tabbar_home@3x.png" alt="特价商品">
            <div class="name-c">
              <span class="name">商品名称</span>
            </div>
            <span class="price">¥9.9</span>
          </div>
          <div class="deal-item" @click="viewProductDetail">
            <img src="/static/images/icon_tabbar_home@3x.png" alt="特价商品">
            <div class="name-c">
              <span class="name">商品名称</span>
            </div>
            <span class="price">¥9.9</span>
          </div>
          <div class="deal-item" @click="viewProductDetail">
            <img src="/static/images/icon_tabbar_home@3x.png" alt="特价商品">
            <div class="name-c">
              <span class="name">商品名称</span>
            </div>
            <span class="price">¥9.9</span>
          </div>
        </div>
        <button class="view-all" @click="viewAllDeals">查看全部</button>
      </div>
      
      <!-- 分类标签 -->
      <div class="category-tabs">
        <ul>
          <li class="active">精选</li>
          <li>24h最热</li>
          <li>3h最热</li>
          <li>好价活动</li>
          <li>食品</li>
          <li>居家</li>
        </ul>
      </div>
      
      <!-- 商品列表 -->
      <div class="product-list">
        <!-- 示例商品，实际应从数据渲染 -->
        <div class="product-item" @click="viewProductDetail">
          <img src="/static/images/icon_tabbar_home@3x.png" alt="商品">
          <div class="name-c">
            <span class="name">商品名称</span>
          </div>
          <span class="price">¥19.9</span>
        </div>
        <div class="product-item" @click="viewProductDetail">
          <img src="/static/images/icon_tabbar_home@3x.png" alt="商品">
          <div class="name-c">
            <span class="name">商品名称</span>
          </div>
          <span class="price">¥19.9</span>
        </div>
        <div class="product-item" @click="viewProductDetail">
          <img src="/static/images/icon_tabbar_home@3x.png" alt="商品">
          <div class="name-c">
            <span class="name">商品名称</span>
          </div>
          <span class="price">¥19.9</span>
        </div>
        <!-- 更多商品... -->
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
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    viewProductDetail() {
      // 跳转到商品详情页，这里需要根据实际路由调整
      wx.navigateTo({
        url: '/pages/productDetail/main'
      });
    },
    viewAllDeals() {
      // 查看全部低价商品
      wx.navigateTo({
        url: '/pages/allDeals/main'
      });
    },
    recommendProduct() {
      // 推荐低价商品
      wx.navigateTo({
        url: '/pages/recommendProduct/main'
      });
    }
  }
};
</script>

<style lang="scss">
@import "@/assets/global.scss";

.container {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  box-sizing: border-box;
  background-color: $page-bgcolor;
}

.content {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.search-box {
  display: flex;
  align-items: center;
  padding: 20rpx;
  background-color: white;
  border-bottom: 1rpx solid $spLine-color;
  
  .icon {
    font-size: 28rpx;
    color: $textGray-color;
    margin-right: 20rpx;
  }
  
  input {
    flex: 1;
    height: 40rpx;
    font-size: 24rpx;
    color: $textBlack-color;
    background-color: $page-bgcolor;
    border-radius: 20rpx;
    padding: 0 20rpx;
  }
}

.daily-deals {
  background-color: white;
  padding: 20rpx;
  margin-bottom: 10rpx;
  border-radius: 8rpx;
  box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.05);
  
  .section {
    display: flex;
    align-items: center;
    justify-content: center;
    margin-bottom: 20rpx;
    
    .l, .r {
      height: 2rpx;
      width: 60rpx;
      background-color: $spLine-color;
    }
    
    .m {
      font-size: 28rpx;
      color: $textBlack-color;
      margin: 0 20rpx;
      font-weight: bold;
    }
  }
  
  .deal-items {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20rpx;
    
    .deal-item {
      width: 30%;
      display: flex;
      flex-direction: column;
      align-items: center;
      background-color: $page-bgcolor;
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
    background-color: $theme-color;
    color: white;
    border: none;
    border-radius: 30rpx;
    font-size: 24rpx;
    font-weight: bold;
  }
}

.category-tabs {
  background-color: white;
  padding: 0 20rpx;
  margin-bottom: 10rpx;
  border-radius: 8rpx;
  box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.05);
  
  ul {
    display: flex;
    justify-content: space-between;
    padding: 20rpx 0;
    
    li {
      font-size: 24rpx;
      color: $textDarkGray-color;
      padding: 10rpx 20rpx;
      border-radius: 20rpx;
      
      &.active {
        background-color: $theme-color;
        color: white;
      }
    }
  }
}

.product-list {
  background-color: white;
  padding: 20rpx;
  margin-bottom: 10rpx;
  border-radius: 8rpx;
  box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.05);
  
  .product-item {
    display: flex;
    margin-bottom: 20rpx;
    padding-bottom: 20rpx;
    border-bottom: 1rpx solid $spLine-color;
    
    &:last-child {
      border-bottom: none;
      margin-bottom: 0;
      padding-bottom: 0;
    }
    
    img {
      width: 120rpx;
      height: 120rpx;
      border-radius: 8rpx;
      margin-right: 20rpx;
    }
    
    .name-c {
      flex: 1;
      margin-bottom: 10rpx;
      
      .name {
        font-size: 24rpx;
        color: $textBlack-color;
        line-height: 1.5;
      }
    }
    
    .price {
      font-size: 28rpx;
      color: $mtRed-color;
      font-weight: bold;
    }
  }
}

.my-exposure {
  background-color: white;
  padding: 20rpx;
  margin-bottom: 10rpx;
  border-radius: 8rpx;
  box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.05);
  
  .section {
    display: flex;
    align-items: center;
    justify-content: center;
    margin-bottom: 20rpx;
    
    .l, .r {
      height: 2rpx;
      width: 60rpx;
      background-color: $spLine-color;
    }
    
    .m {
      font-size: 28rpx;
      color: $textBlack-color;
      margin: 0 20rpx;
      font-weight: bold;
    }
  }
  
  .recommend-btn {
    width: 100%;
    height: 60rpx;
    background-color: $theme-color;
    color: white;
    border: none;
    border-radius: 30rpx;
    font-size: 24rpx;
    font-weight: bold;
  }
}
</style>