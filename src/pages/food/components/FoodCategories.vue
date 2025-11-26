<template>
  <div class="food-categories">
    <div class="category-tabs">
      <div class="tab-item" v-for="(category, index) in categories" :key="index"
           :class="{active: activeCategory === index}" @click="onCategoryClick(index)">
        {{ category.name }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'FoodCategories',
  data() {
    return {
      categories: [
        { name: '全部美食' },
        { name: '人气推荐' },
        { name: '限时秒杀' },
        { name: '学生专区' },
        { name: '美食排行' }
      ],
      activeCategory: 0
    }
  },
  methods: {
    onCategoryClick(index) {
      this.activeCategory = index;
      // Emit event to parent component
      this.$emit('categoryChange', index);
    }
  }
}
</script>

<style lang="scss" scoped>
.food-categories {
  background-color: #fff;
  border-bottom: 1px solid #f0f0f0;
  
  .category-tabs {
    display: flex;
    overflow-x: auto;
    padding: 0 20rpx;
    
    .tab-item {
      padding: 20rpx 30rpx;
      font-size: 28rpx;
      color: #666;
      white-space: nowrap;
      transition: color 0.3s ease;
      
      &.active {
        color: #ff4444;
        font-weight: bold;
        border-bottom: 3px solid #ff4444;
      }
      
      &:not(:last-child) {
        margin-right: 20rpx;
      }
    }
  }
}
</style>