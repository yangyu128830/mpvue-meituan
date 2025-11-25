<template>
  <div class="container">
    <div class="content">
      <!-- Search Bar -->
      <div class="search-bar">
        <div class="search-input" @click="searchClick">
          <i class="icon mt-search-o"></i>
          <input 
            type="text" 
            placeholder="搜索药品、医生..." 
            v-model="searchQuery"
            @input="search"
            @keyup.enter="search"
          />
        </div>
      </div>

      <!-- Search Results -->
      <div class="search-results" v-if="showSearchResults && searchResults.length > 0">
        <div class="section">
          <div class="l"></div>
          <span class="m">搜索结果</span>
          <div class="r"></div>
        </div>
        
        <!-- Medicine Results -->
        <div class="medicine-results" v-if="searchResults.filter(item => item.type === 'medicine').length > 0">
          <div class="section">
            <div class="l"></div>
            <span class="m">药品</span>
            <div class="r"></div>
          </div>
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
          <div class="section">
            <div class="l"></div>
            <span class="m">医生</span>
            <div class="r"></div>
          </div>
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
        <div class="section">
          <div class="l"></div>
          <span class="m">热门药品</span>
          <div class="r"></div>
        </div>
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
        { id: 1, name: '布洛芬缓释胶囊', description: '缓解疼痛、退烧', price: 25.9, image: '/static/images/icon_medicine.svg' },
        { id: 2, name: '连花清瘟胶囊', description: '清热解毒', price: 36.8, image: '/static/images/icon_medicine.svg' },
        { id: 3, name: '藿香正气水', description: '防暑降温', price: 12.5, image: '/static/images/icon_medicine.svg' }
      ],
      searchQuery: '',
      searchResults: [],
      showSearchResults: false
    }
  },
  methods: {
    searchClick() {
      // For now, focus the input when clicked
      const input = document.querySelector('.search-input input')
      if (input) {
        input.focus()
      }
    },
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
      const query = this.searchQuery.toLowerCase()
      this.searchResults = allResults.filter(item => 
        item.name.toLowerCase().includes(query) || 
        (item.description && item.description.toLowerCase().includes(query)) || 
        (item.specialty && item.specialty.toLowerCase().includes(query)) || 
        (item.hospital && item.hospital.toLowerCase().includes(query))
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

<style lang="scss" scoped>
@import "@/assets/global.scss";

.container {
  .content {
    display: flex;
    flex-direction: column;
    position: relative;
    background-color: $page-bgcolor;
    
    .search-bar {
      display: flex;
      align-items: center;
      height: 80rpx;
      padding: 0 30rpx;
      width: 100%;
      box-sizing: border-box;
      background-color: white;
      margin-bottom: 20rpx;
      
      .search-input {
        display: flex;
        align-items: center;
        flex: 1;
        background-color: $page-bgcolor;
        height: 60rpx;;
        border-radius: 30rpx;
        align-items: center;
        
        i {
          color: $textDarkGray-color;
          font-size: 32rpx;
          margin-left: 20rpx;
        }
        
        input {
          color: $textDarkGray-color;
          font-size: 24rpx;
          margin-left: 10rpx;
          border: none;
          flex: 1;
          background-color: transparent;
          padding-right: 20rpx;
          
          &::placeholder {
            color: $placeholder-textcolor;
            font-size: 24rpx;
          }
        }
      }
    }

    .search-results {
      margin-bottom: 20rpx;
      background-color: white;
      margin: 0 20rpx;
      border-radius: 8rpx;
      padding: 20rpx;
    }

    .no-results {
      text-align: center;
      padding: 40rpx 20rpx;
      color: $textDarkGray-color;
      background-color: white;
      margin: 0 20rpx;
      border-radius: 8rpx;
    }

    .section {
      display: flex;
      align-items: center;
      margin-bottom: 20rpx;
      
      .l, .r {
        flex: 1;
        height: 2rpx;
        background-color: $spLine-color;
      }
      
      .m {
        font-size: 28rpx;
        color: $textBlack-color;
        margin: 0 20rpx;
      }
    }

    .doctor-list {
      display: flex;
      flex-direction: column;
      gap: 20rpx;
    }

    .doctor-item {
      display: flex;
      align-items: center;
      padding: 20rpx;
      background-color: white;
      border-radius: 8rpx;
      
      img {
        width: 120rpx;
        height: 120rpx;
        border-radius: 50%;
        margin-right: 20rpx;
      }

      .doctor-info {
        flex: 1;
        
        h4 {
          font-size: 32rpx;
          margin-bottom: 10rpx;
          color: $textBlack-color;
        }

        p {
          font-size: 28rpx;
          margin-bottom: 10rpx;
          color: $textDarkGray-color;
        }

        .hospital {
          font-size: 24rpx;
          color: $textGray-color;
          margin-right: 20rpx;
        }

        .rating {
          font-size: 24rpx;
          color: $theme-color;
        }
      }

      .consult-btn {
        padding: 16rpx 24rpx;
        background-color: $theme-color;
        color: white;
        border: none;
        border-radius: 30rpx;
        cursor: pointer;
        font-size: 24rpx;
      }
    }

    .tab-switcher {
      display: flex;
      background-color: white;
      margin: 0 20rpx;
      border-radius: 8rpx;
      overflow: hidden;
    }

    .tab-item {
      flex: 1;
      text-align: center;
      padding: 20rpx 0;
      font-size: 28rpx;
      color: $textDarkGray-color;
      cursor: pointer;
      border-bottom: 4rpx solid transparent;
      
      &.active {
        color: $theme-color;
        border-bottom: 4rpx solid $theme-color;
      }
    }

    .tab-content {
      margin-bottom: 20rpx;
      background-color: white;
      margin: 0 20rpx;
      border-radius: 8rpx;
      padding: 20rpx;
    }

    .hot-medicines {
      background-color: white;
      margin: 0 20rpx;
      border-radius: 8rpx;
      padding: 20rpx;
      margin-bottom: 20rpx;
    }

    .medicine-list {
      display: flex;
      flex-wrap: wrap;
      gap: 20rpx;
    }

    .medicine-item {
      width: calc(50% - 10rpx);
      background-color: white;
      border-radius: 8rpx;
      padding: 20rpx;
      box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.05);
      
      img {
        width: 100%;
        height: 160rpx;
        object-fit: contain;
        border-radius: 8rpx;
        margin-bottom: 10rpx;
      }

      .medicine-info {
        h4 {
          font-size: 28rpx;
          margin-bottom: 10rpx;
          color: $textBlack-color;
        }

        p {
          font-size: 24rpx;
          color: $textDarkGray-color;
          margin-bottom: 10rpx;
          line-height: 1.4;
        }

        .price {
          font-size: 32rpx;
          color: $mtRed-color;
          font-weight: bold;
          display: block;
          margin-bottom: 10rpx;
        }

        .add-to-cart {
          width: 100%;
          padding: 16rpx 0;
          background-color: $theme-color;
          color: white;
          border: none;
          border-radius: 30rpx;
          cursor: pointer;
          font-size: 24rpx;
        }
      }
    }
  }
}
</style>