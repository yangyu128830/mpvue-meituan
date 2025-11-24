<template>
  <div class="medical-page">
    <!-- Search Bar -->
    <div class="search-bar">
      <input type="text" placeholder="搜索药品、医生、症状" />
      <button class="search-btn">搜索</button>
    </div>

    <!-- Tab Switcher -->
    <div class="tab-container">
      <div 
        class="tab-item" 
        v-for="(tab, index) in tabs" 
        :key="index"
        :class="{ active: activeTab === index }"
        @click="switchTab(index)"
      >
        {{ tab.name }}
      </div>
    </div>

    <!-- Content Area -->
    <div class="content-area">
      <div v-if="activeTab === 0">
        <recommend-page />
      </div>
      <div v-else-if="activeTab === 1">
        <ask-doctor-page />
      </div>
      <div v-else-if="activeTab === 2">
        <physical-test-page />
      </div>
    </div>

    <!-- Hot Medicine Recommendations (Fixed at Bottom) -->
    <div class="hot-medicine-container">
      <h3>热门药品推荐</h3>
      <div class="medicine-list">
        <div class="medicine-item" v-for="item in hotMedicines" :key="item.id">
          <img :src="item.image" alt="{{ item.name }}" />
          <div class="medicine-info">
            <h4>{{ item.name }}</h4>
            <p>{{ item.description }}</p>
            <span class="price">¥{{ item.price }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import RecommendPage from './recommend/main'
import AskDoctorPage from './askDoctor/main'
import PhysicalTestPage from './physicalTest/main'

export default {
  components: {
    RecommendPage,
    AskDoctorPage,
    PhysicalTestPage
  },
  data() {
    return {
      activeTab: 0,
      tabs: [
        { name: '推荐' },
        { name: '问医生' },
        { name: '做体测' }
      ],
      hotMedicines: [
        { id: 1, name: '布洛芬缓释胶囊', description: '缓解疼痛、退烧', price: 25.9, image: 'https://via.placeholder.com/100' },
        { id: 2, name: '连花清瘟胶囊', description: '清热解毒', price: 36.8, image: 'https://via.placeholder.com/100' },
        { id: 3, name: '藿香正气水', description: '防暑降温', price: 12.5, image: 'https://via.placeholder.com/100' }
      ]
    }
  },
  methods: {
    switchTab(index) {
      this.activeTab = index
    }
  }
}
</script>

<style scoped>
.medical-page {
  padding-bottom: 100px;
}

.search-bar {
  display: flex;
  padding: 10px;
  background-color: #f5f5f5;
}

.search-bar input {
  flex: 1;
  padding: 8px 12px;
  border: 1px solid #ddd;
  border-radius: 4px 0 0 4px;
  font-size: 14px;
}

.search-btn {
  padding: 8px 16px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 0 4px 4px 0;
  cursor: pointer;
}

.tab-container {
  display: flex;
  justify-content: space-around;
  padding: 10px 0;
  border-bottom: 1px solid #eee;
}

.tab-item {
  padding: 8px 16px;
  font-size: 14px;
  cursor: pointer;
}

.tab-item.active {
  color: #007bff;
  border-bottom: 2px solid #007bff;
}

.content-area {
  padding: 10px;
}

.hot-medicine-container {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: white;
  border-top: 1px solid #eee;
  padding: 10px;
}

.hot-medicine-container h3 {
  font-size: 16px;
  margin-bottom: 10px;
}

.medicine-list {
  display: flex;
  overflow-x: auto;
}

.medicine-item {
  margin-right: 10px;
  width: 120px;
}

.medicine-item img {
  width: 100%;
  height: 100px;
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
}
</style>