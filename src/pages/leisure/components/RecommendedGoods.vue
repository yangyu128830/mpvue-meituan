<template>
  <div class="recommended-goods">
    <div class="section-header">
      <div class="section-title">推荐商品</div>
    </div>
    
    <div class="filter-bar">
      <div 
        class="filter-item" 
        :class="{ active: activeFilter === filter.value }"
        v-for="(filter, index) in filters"
        :key="index"
        @click="onFilterClick(filter.value)"
      >
        {{ filter.label }}
      </div>
    </div>
    
    <div class="goods-list">
      <div class="goods-item" v-for="(goods, index) in recommendedGoods" :key="index">
        <div class="goods-image">
          <img :src="goods.image" alt="Goods image" />
          <span class="tag" v-if="goods.tag">{{ goods.tag }}</span>
        </div>
        <div class="goods-details">
          <div class="goods-name">{{ goods.name }}</div>
          <div class="goods-info">
            <span class="rating"><i class="icon mt-star-o"></i>{{ goods.rating }}</span>
            <span class="sales">销量{{ goods.sales }}</span>
            <span class="distance">{{ goods.distance }}</span>
          </div>
          <div class="price-info">
            <span class="price">¥{{ goods.price }}</span>
            <span class="original-price" v-if="goods.originalPrice">¥{{ goods.originalPrice }}</span>
          </div>
          <div class="tags">
            <span class="tag-item" v-for="(tag, index) in goods.tags" :key="index">{{ tag }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'RecommendedGoods',
  props: {
    recommendedGoods: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      activeFilter: 'sales',
      filters: [
        { label: '销量优先', value: 'sales' },
        { label: '距离最近', value: 'distance' },
        { label: '评分最高', value: 'rating' },
        { label: '价格最低', value: 'price' }
      ]
    };
  },
  methods: {
    onFilterClick(filter) {
      this.activeFilter = filter;
      this.$emit('filterChange', filter);
    }
  }
};
</script>

<style lang="scss" scoped>
.recommended-goods {
  background-color: white;
  
  .section-header {
    padding: 16rpx 24rpx;
    border-bottom: 1rpx solid #f5f5f5;
    
    .section-title {
      font-size: 28rpx;
      color: #333;
      font-weight: bold;
    }
  }
  
  .filter-bar {
    display: flex;
    overflow-x: auto;
    padding: 16rpx 24rpx;
    border-bottom: 1rpx solid #f5f5f5;
    
    .filter-item {
      flex: 0 0 auto;
      margin-right: 32rpx;
      font-size: 24rpx;
      color: #666;
      
      &.active {
        color: #ff6700;
      }
    }
  }
  
  .goods-list {
    padding: 16rpx;
    
    .goods-item {
      display: flex;
      margin-bottom: 16rpx;
      
      .goods-image {
        width: 180rpx;
        height: 180rpx;
        margin-right: 16rpx;
        position: relative;
        
        img {
          width: 100%;
          height: 100%;
          border-radius: 8rpx;
          object-fit: cover;
        }
        
        .tag {
          position: absolute;
          top: 8rpx;
          left: 8rpx;
          background-color: #ff6700;
          color: white;
          font-size: 16rpx;
          padding: 4rpx 8rpx;
          border-radius: 4rpx;
        }
      }
      
      .goods-details {
        flex: 1;
        display: flex;
        flex-direction: column;
        
        .goods-name {
          font-size: 28rpx;
          color: #333;
          margin-bottom: 8rpx;
          line-height: 1.4;
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
        }
        
        .goods-info {
          display: flex;
          align-items: center;
          margin-bottom: 8rpx;
          font-size: 20rpx;
          color: #999;
          
          span {
            margin-right: 16rpx;
            
            i {
              color: #ffd26b;
              margin-right: 4rpx;
            }
          }
        }
        
        .price-info {
          display: flex;
          align-items: baseline;
          margin-bottom: 8rpx;
          
          .price {
            font-size: 28rpx;
            color: #ff6700;
            font-weight: bold;
            margin-right: 8rpx;
          }
          
          .original-price {
            font-size: 20rpx;
            color: #999;
            text-decoration: line-through;
          }
        }
        
        .tags {
          display: flex;
          flex-wrap: wrap;
          
          .tag-item {
            background-color: #f5f5f5;
            color: #666;
            font-size: 20rpx;
            padding: 4rpx 8rpx;
            border-radius: 4rpx;
            margin-right: 8rpx;
            margin-bottom: 8rpx;
          }
        }
      }
    }
  }
}
</style>