<template>
  <div class="recommend-page">
    <!-- Basic Medicine Recommendations -->
    <div class="medicine-section">
      <h3>基础药品推荐</h3>
      <div class="medicine-list">
        <div class="medicine-item" v-for="item in basicMedicines" :key="item.id">
          <img :src="item.image" alt="{{ item.name }}" />
          <div class="medicine-info">
            <h4>{{ item.name }}</h4>
            <p>{{ item.description }}</p>
            <span class="price">¥{{ item.price }}</span>
            <button class="add-to-cart">加入购物车</button>
          </div>
        </div>
      </div>
    </div>

    <!-- Frequently Bought Medicines -->
    <div class="medicine-section">
      <h3>常买药品</h3>
      <div class="medicine-list">
        <div class="medicine-item" v-for="item in frequentMedicines" :key="item.id">
          <img :src="item.image" alt="{{ item.name }}" />
          <div class="medicine-info">
            <h4>{{ item.name }}</h4>
            <p>{{ item.description }}</p>
            <span class="price">¥{{ item.price }}</span>
            <button class="add-to-cart">加入购物车</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      basicMedicines: [
        { id: 1, name: '创可贴', description: '止血、防感染', price: 5.9, image: 'https://via.placeholder.com/150' },
        { id: 2, name: '碘伏消毒液', description: '伤口消毒', price: 8.5, image: 'https://via.placeholder.com/150' },
        { id: 3, name: '体温计', description: '测量体温', price: 22.8, image: 'https://via.placeholder.com/150' },
        { id: 4, name: '感冒灵颗粒', description: '缓解感冒症状', price: 16.9, image: 'https://via.placeholder.com/150' }
      ],
      frequentMedicines: [
        { id: 1, name: '维生素C片', description: '补充维生素C', price: 19.9, image: 'https://via.placeholder.com/150' },
        { id: 2, name: '钙片', description: '补钙', price: 45.6, image: 'https://via.placeholder.com/150' },
        { id: 3, name: '润喉糖', description: '缓解喉咙不适', price: 12.3, image: 'https://via.placeholder.com/150' },
        { id: 4, name: '眼药水', description: '缓解眼疲劳', price: 28.7, image: 'https://via.placeholder.com/150' }
      ]
    }
  }
}
</script>

<style scoped>
.recommend-page {
  padding: 10px;
}

.medicine-section {
  margin-bottom: 20px;
}

.medicine-section h3 {
  font-size: 16px;
  margin-bottom: 10px;
  padding-bottom: 5px;
  border-bottom: 1px solid #eee;
}

.medicine-list {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

.medicine-item {
  width: calc(50% - 5px);
  background-color: #fff;
  border: 1px solid #eee;
  border-radius: 4px;
  padding: 10px;
}

.medicine-item img {
  width: 100%;
  height: 120px;
  object-fit: cover;
  border-radius: 4px;
}

.medicine-info h4 {
  font-size: 14px;
  margin: 5px 0;
}

.medicine-info p {
  font-size: 12px;
  color: #666;
  margin: 5px 0;
}

.medicine-info .price {
  font-size: 14px;
  color: #f00;
  font-weight: bold;
  display: block;
  margin: 5px 0;
}

.add-to-cart {
  width: 100%;
  padding: 6px 0;
  background-color: #ff6600;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 12px;
}
</style>