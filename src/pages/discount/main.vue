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
      // 跳转到全部特价商品页
      wx.navigateTo({
        url: '/pages/discount/allDeals'
      });
    },
    recommendProduct() {
      // 跳转到推荐商品页
      wx.navigateTo({
        url: '/pages/discount/recommend'
      });
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  .content {
    display: flex;
    flex-direction: column;
    position: relative;
    background-color: $page-bgcolor;
    
    .search-box {
      display: flex;
      align-items: center;
      background-color: white;
      height: 80rpx;
      padding: 0 30rpx;
      border-bottom: $line-width solid $spLine-color;
      box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.05);
      
      i {
        color: $textDarkGray-color;
        font-size: 32rpx;
      }
      
      input {
        flex: 1;
        background-color: $page-bgcolor;
        height: 60rpx;
        border-radius: 30rpx;
        margin-left: 20rpx;
        padding: 0 20rpx;
        font-size: 24rpx;
        color: $textBlack-color;
        border: none;
        outline: none;
        &::placeholder {
          color: $placeholder-textcolor;
          font-size: $placeholder-font * 2 + rpx;
        }
      }
    }
    
    .daily-deals {
      margin: 20rpx;
      background-color: white;
      border-radius: 8rpx;
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
          background-color: $spLine-color;
        }
        
        .m {
          font-size: 32rpx;
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
          width: 32%;
          display: flex;
          flex-direction: column;
          align-items: center;
          background-color: #fafafa;
          padding: 10rpx;
          border-radius: 8rpx;
          transition: all 0.3s ease;
          
          &:hover {
            transform: translateY(-2rpx);
            box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.1);
          }
          
          img {
            width: 160rpx;
            height: 120rpx;
            border-radius: 8rpx;
            margin-bottom: 10rpx;
          }
          
          .name-c {
            display: flex;
            justify-content: center;
            align-items: center;
            margin-bottom: 8rpx;
            
            .name {
              font-size: 24rpx;
              color: $textBlack-color;
              text-align: center;
              white-space: nowrap;
              overflow: hidden;
              text-overflow: ellipsis;
            }
          }
          
          .price {
            font-size: 28rpx;
            color: $mtRed-color;
            font-weight: bold;
          }
        }
      }
      
      .view-all {
        width: 100%;
        height: 80rpx;
        background-color: $theme-color;
        color: white;
        border: none;
        border-radius: 8rpx;
        font-size: 28rpx;
        font-weight: bold;
        transition: all 0.3s ease;
        
        &:hover {
          background-color: darken($theme-color, 10%);
        }
      }
    }
    
    .category-tabs {
      background-color: white;
      padding: 20rpx;
      margin-bottom: 20rpx;
      box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.05);
      
      ul {
        display: flex;
        overflow-x: auto;
        padding-bottom: 10rpx;
        
        li {
          font-size: 28rpx;
          color: $textDarkGray-color;
          padding: 10rpx 20rpx;
          margin-right: 20rpx;
          border-radius: 20rpx;
          white-space: nowrap;
          transition: all 0.3s ease;
          
          &:last-child {
            margin-right: 0;
          }
          
          &.active {
            background-color: $theme-color;
            color: white;
          }
          
          &:hover:not(.active) {
            background-color: #f0f0f0;
          }
        }
      }
    }
    
    .product-list {
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      padding: 0 20rpx 20rpx;
      
      .product-item {
        width: 48%;
        background-color: white;
        border-radius: 8rpx;
        margin-bottom: 20rpx;
        padding: 10rpx;
        box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.05);
        transition: all 0.3s ease;
        
        &:hover {
          transform: translateY(-2rpx);
          box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.1);
        }
        
        img {
          width: 100%;
          height: 240rpx;
          border-radius: 8rpx;
          margin-bottom: 10rpx;
        }
        
        .name-c {
          display: flex;
          justify-content: center;
          align-items: center;
          margin-bottom: 8rpx;
          
          .name {
            font-size: 24rpx;
            color: $textBlack-color;
            text-align: center;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
          }
        }
        
        .price {
          font-size: 28rpx;
          color: $mtRed-color;
          font-weight: bold;
          text-align: center;
        }
      }
    }
    
    .my-exposure {
      background-color: white;
      padding: 20rpx;
      box-shadow: 0 -2rpx 10rpx rgba(0, 0, 0, 0.05);
      
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
          font-size: 32rpx;
          color: $textBlack-color;
          margin: 0 20rpx;
          font-weight: bold;
        }
      }
      
      .recommend-btn {
        width: 100%;
        height: 80rpx;
        background-color: $theme-color;
        color: white;
        border: none;
        border-radius: 8rpx;
        font-size: 28rpx;
        font-weight: bold;
        transition: all 0.3s ease;
        
        &:hover {
          background-color: darken($theme-color, 10%);
        }
      }
    }
  }
}
</style>
          }
          
          .name-c {
            display: flex;
            align-items: center;
            position: relative;
            height: 30rpx;
            margin: 10rpx 0;
            .name {
              font-size: 20rpx;
              color: $textBlack-color;
              overflow: hidden;
              text-overflow: ellipsis;
              white-space: nowrap;
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
        display: block;
        width: 100%;
        padding: 15rpx;
        background-color: white;
        border: 2rpx solid $mtRed-color;
        color: $mtRed-color;
        border-radius: 8rpx;
        font-size: 24rpx;
        cursor: pointer;
      }
    }
    
    .category-tabs {
      margin: 0 20rpx 20rpx;
      background-color: white;
      border-radius: 8rpx;
      padding: 20rpx;
      
      ul {
        display: flex;
        list-style: none;
        padding: 0;
        margin: 0;
        overflow-x: auto;
        
        li {
          margin-right: 20rpx;
          padding: 10rpx 20rpx;
          border-radius: 30rpx;
          background-color: $page-bgcolor;
          font-size: 24rpx;
          color: $textDarkGray-color;
          cursor: pointer;
          white-space: nowrap;
        }
        
        li.active {
          background-color: $mtRed-color;
          color: white;
        }
      }
    }
    
    .product-list {
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      margin: 0 20rpx 20rpx;
      
      .product-item {
        width: 48%;
        background-color: white;
        border-radius: 8rpx;
        padding: 10rpx;
        margin-bottom: 20rpx;
        display: flex;
        flex-direction: column;
        
        img {
          width: 100%;
          height: 120rpx;
          border-radius: 8rpx;
        }
        
        .name-c {
          display: flex;
          align-items: center;
          position: relative;
          height: 30rpx;
          margin: 10rpx 0;
          
          .name {
            font-size: 20rpx;
            color: $textBlack-color;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
          }
        }
        
        .price {
          font-size: 24rpx;
          color: $mtRed-color;
          font-weight: bold;
        }
      }
    }
    
    .my-exposure {
      margin: 0 20rpx 20rpx;
      background-color: white;
      border-radius: 8rpx;
      padding: 20rpx;
      
      .section {
        display: flex;
        align-items: center;
        justify-content: center;
        margin-bottom: 20rpx;
        
        .l {
          height: 2rpx;
          width: 60rpx;
          background-color: $textDarkGray-color;
        }
        
        .m {
          font-size: 32rpx;
          color: $textBlack-color;
          margin: 0 20rpx;
          font-weight: bold;
        }
        
        .r {
          height: 2rpx;
          width: 60rpx;
          background-color: $textDarkGray-color;
        }
      }
      
      .recommend-btn {
        display: block;
        width: 100%;
        padding: 15rpx;
        background-color: $mtRed-color;
        color: white;
        border: none;
        border-radius: 8rpx;
        font-size: 24rpx;
        cursor: pointer;
      }
    }
  }
}
</style>