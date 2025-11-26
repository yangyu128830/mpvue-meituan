<template>
  <div class="bath-category">
    <div class="category-content">
      <!-- 洗浴汗蒸分类的内容 -->
      <div class="section-header">
        <div class="section-title">洗浴汗蒸</div>
      </div>
      
      <div class="goods-list">
        <div class="goods-item" v-for="(goods, index) in bathGoods" :key="index">
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
  </div>
</template>

<script>
export default {
  name: 'BathCategory',
  props: {
    bathGoods: {
      type: Array,
      required: true
    }
  }
};
</script>

<style lang="scss" scoped>
.bath-category {
  background-color: #f5f5f5;
  min-height: 100vh;
}
</style>