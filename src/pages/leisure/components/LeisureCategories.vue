<template>
  <div class="leisure-categories">
    <div class="category-tabs">
      <div 
        class="tab-item" 
        :class="{ active: activeCategory === category.value }"
        v-for="(category, index) in categories"
        :key="index"
        @click="onTabClick(category.value)"
      >
        {{ category.label }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'LeisureCategories',
  data() {
    return {
      activeCategory: 'recommend',
      categories: [
        { label: '推荐', value: 'recommend' },
        { label: '按摩足浴', value: 'massage' },
        { label: '洗浴汗蒸', value: 'bath' },
        { label: 'KTV', value: 'ktv' }
      ]
    };
  },
  methods: {
    onTabClick(category) {
      this.activeCategory = category;
      this.$emit('categoryChange', category);
    }
  }
};
</script>

<style lang="scss" scoped>
.leisure-categories {
  background-color: white;
  margin-bottom: 16rpx;
  
  .category-tabs {
    display: flex;
    overflow-x: auto;
    
    .tab-item {
      flex: 1;
      text-align: center;
      padding: 20rpx 0;
      font-size: 28rpx;
      color: #666;
      border-bottom: 4rpx solid transparent;
      
      &.active {
        color: #ff6700;
        border-bottom-color: #ff6700;
      }
    }
  }
}
</style>