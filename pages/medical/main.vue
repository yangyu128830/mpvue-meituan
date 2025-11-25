<template>
  <div class="medical-page">
    <!-- Search Bar -->
    <div class="search-bar">
      <input 
        type="text" 
        placeholder="搜索药品、医生..." 
        v-model="searchQuery"
        @input="search"
        @keyup.enter="search"
      />
      <button class="search-btn" @click="search">搜索</button>
    </div>

    <!-- Search Results -->
    <div class="search-results" v-if="showSearchResults && searchResults.length > 0">
      <h3>搜索结果</h3>
      
      <!-- Medicine Results -->
      <div class="medicine-results" v-if="searchResults.filter(item => item.type === 'medicine').length > 0">
        <h4>药品</h4>
        <div class="medicine-list">
          <div class="medicine-item" v-for="item in searchResults.filter(item => item.type === 'medicine')" :key="item.id">
            <img :src="item.image" alt="{{ item.name }}" />
            <div class="medicine-info">
              <h4>{{ item.name }}</h4>
              <p>{{ item.description }}</p>
              <span class="price">¥{{ item.price }}</span>
              <button class="add-to-cart" @click="addToCart(item)">加入购物车</button>
            </div>
          </div>
        </div>
      </div>

      <!-- Doctor Results -->
      <div class="doctor-results" v-if="searchResults.filter(item => item.type === 'doctor').length > 0">
        <h4>医生</h4>
        <div class="doctor-list">
          <div class="doctor-item" v-for="item in searchResults.filter(item => item.type === 'doctor')" :key="item.id">
            <img :src="item.avatar" alt="{{ item.name }}" />
            <div class="doctor-info">
              <h4>{{ item.name }}</h4>
              <p>{{ item.specialty }}</p>
              <span class="hospital">{{ item.hospital }}</span>
              <span class="rating">★★★★★ ({{ item.rating }})</span>
            </div>
            <button class="consult-btn" @click="startConsultation(item)">在线咨询</button>
          </div>
        </div>
      </div>
    </div>

    <!-- No Search Results -->
    <div class="no-results" v-if="showSearchResults && searchResults.length === 0">
      <p>未找到相关结果，请尝试其他关键词</p>
    </div>

    <!-- Tab Switcher -->
    <div class="tab-switcher">
      <div class="tab-item" v-for="(tab, index) in tabs" :key="index" :class="{ active: activeTab === index }" @click="switchTab(index)">
        {{ tab.name }}
      </div>
    </div>

    <!-- Tab Content -->
    <div class="tab-content">
      <recommend-page v-show="activeTab === 0" ref="recommendPage" />
      <ask-doctor-page v-show="activeTab === 1" ref="askDoctorPage" />
      <physical-test-page v-show="activeTab === 2" />
    </div>

    <!-- Hot Medicines -->
    <div class="hot-medicines">
      <h3>热门药品</h3>
      <div class="medicine-list">
        <div class="medicine-item" v-for="item in hotMedicines" :key="item.id">
          <img :src="item.image" alt="{{ item.name }}" />
          <div class="medicine-info">
            <h4>{{ item.name }}</h4>
            <p>{{ item.description }}</p>
            <span class="price">¥{{ item.price }}</span>
            <button class="add-to-cart" @click="addToCart(item)">加入购物车</button>
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
import store from '@/store'

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
      ],
      searchQuery: '',
      searchResults: [],
      showSearchResults: false
    }
  },
  methods: {
    switchTab(index) {
      this.activeTab = index
      this.showSearchResults = false
    },
    search() {
      if (!this.searchQuery.trim()) {
        this.showSearchResults = false
        return
      }

      // Combine all medicine and doctor data for search
      const recommendPage = this.$refs.recommendPage
      const askDoctorPage = this.$refs.askDoctorPage
      const allResults = []

      if (recommendPage) {
        const allMedicines = [...recommendPage.basicMedicines, ...recommendPage.frequentMedicines, ...this.hotMedicines]
        allMedicines.forEach(medicine => {
          allResults.push({ ...medicine, type: 'medicine' })
        })
      }

      if (askDoctorPage) {
        askDoctorPage.doctors.forEach(doctor => {
          allResults.push({ ...doctor, type: 'doctor' })
        })
      }

      // Filter results based on search query
      this.searchResults = allResults.filter(item => 
        item.name.includes(this.searchQuery) || 
        (item.description && item.description.includes(this.searchQuery)) || 
        (item.specialty && item.specialty.includes(this.searchQuery)) || 
        (item.hospital && item.hospital.includes(this.searchQuery))
      )

      this.showSearchResults = true
    },
    addToCart(item) {
      const recommendPage = this.$refs.recommendPage
      if (recommendPage) {
        recommendPage.addToCart(item)
      }
    },
    startConsultation(doctor) {
      const askDoctorPage = this.$refs.askDoctorPage
      if (askDoctorPage) {
        askDoctorPage.startConsultation(doctor)
        this.activeTab = 1
        this.showSearchResults = false
      }
    }
  }
}
</script>

<style scoped>
.medical-page {
  padding: 10px;
}

.search-bar {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.search-bar input {
  flex: 1;
  padding: 8px;
  border: 1px solid #eee;
  border-radius: 4px;
  font-size: 14px;
}

.search-bar .search-btn {
  padding: 8px 16px;
  background-color: #ff6600;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}

.search-results {
  margin-bottom: 20px;
}

.search-results h3 {
  font-size: 16px;
  margin-bottom: 10px;
  padding-bottom: 5px;
  border-bottom: 1px solid #eee;
}

.search-results h4 {
  font-size: 14px;
  margin-bottom: 10px;
  color: #666;
}

.no-results {
  text-align: center;
  padding: 40px 20px;
  color: #666;
}

.medicine-results,
.doctor-results {
  margin-bottom: 15px;
}

.doctor-list {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.doctor-item {
  display: flex;
  align-items: center;
  padding: 10px;
  background-color: #fff;
  border: 1px solid #eee;
  border-radius: 4px;
}

.doctor-item img {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  margin-right: 10px;
}

.doctor-info h4 {
  font-size: 14px;
  margin-bottom: 5px;
  color: #000;
}

.doctor-info p {
  font-size: 12px;
  margin-bottom: 5px;
}

.doctor-info .hospital {
  font-size: 12px;
  color: #666;
  margin-right: 10px;
}

.doctor-info .rating {
  font-size: 12px;
  color: #ff6600;
}

.consult-btn {
  margin-left: auto;
  padding: 6px 12px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 12px;
}

.tab-switcher {
  display: flex;
  margin-bottom: 20px;
  border-bottom: 1px solid #eee;
}

.tab-item {
  flex: 1;
  text-align: center;
  padding: 10px 0;
  font-size: 14px;
  color: #666;
  cursor: pointer;
}

.tab-item.active {
  color: #ff6600;
  border-bottom: 2px solid #ff6600;
}

.tab-content {
  margin-bottom: 20px;
}

.hot-medicines {
  margin-top: 20px;
}

.hot-medicines h3 {
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
  width: calc(33.33% - 6.67px);
  background-color: #fff;
  border: 1px solid #eee;
  border-radius: 4px;
  padding: 10px;
}

.medicine-item img {
  width: 100%;
  height: 80px;
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