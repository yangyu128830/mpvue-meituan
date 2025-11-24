<template>
  <div class="hotel-container">
    <!-- 搜索栏 -->
    <div class="search-bar">
      <div class="location-selector" @click="selectLocation">
        <i class="icon mt-location-o"></i>
        <span>{{ currentLocation }}</span>
      </div>
      <div class="search-input-wrapper" @click="showSearchModal">
        <i class="icon mt-search-o"></i>
        <input type="text" placeholder="输入酒店名称或位置" v-model="searchKeyword" @input="onSearchInput">
      </div>
    </div>

    <!-- 时间选择栏 -->
    <div class="date-range-selector">
      <div class="check-in-date" @click="selectCheckInDate">
        <span>入住</span>
        <i class="icon mt-calendar-o"></i>
        <span>{{ checkInDate || '选择日期' }}</span>
      </div>
      <div class="divider">-</div>
      <div class="check-out-date" @click="selectCheckOutDate">
        <span>离店</span>
        <i class="icon mt-calendar-o"></i>
        <span>{{ checkOutDate || '选择日期' }}</span>
      </div>
    </div>

    <!-- 分类标签 -->
    <div class="category-tabs">
      <div class="tab-item" v-for="(category, index) in categories" :key="index" :class="{ active: activeCategory === category.id }" @click="selectCategory(category.id)">
        {{ category.name }}
      </div>
    </div>

    <!-- 主要内容区域 -->
    <div class="main-content">
      <!-- 真便宜 -->
      <section v-if="activeSection === 'cheap'" class="cheap-section">
        <div class="section-header">
          <h2>真便宜</h2>
          <a href="#" class="view-all">查看全部</a>
        </div>
        <div class="hotel-list">
          <div class="hotel-item" v-for="(hotel, index) in cheapHotels" :key="index" @click="viewHotelDetail(hotel.id)">
            <div class="hotel-image">
              <img :src="hotel.imageUrl" alt="Hotel image">
              <div class="tag" v-if="hotel.tag">{{ hotel.tag }}</div>
            </div>
            <div class="hotel-info">
              <h3>{{ hotel.name }}</h3>
              <div class="hotel-meta">
                <span class="rating">{{ hotel.rating }}</span>
                <span class="distance">{{ hotel.distance }}</span>
                <span class="price-range">{{ hotel.priceRange }}</span>
              </div>
              <div class="price">
                <span class="current-price">¥{{ hotel.currentPrice }}</span>
                <span class="original-price" v-if="hotel.originalPrice">¥{{ hotel.originalPrice }}</span>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- 酒店团购 -->
      <section v-if="activeSection === 'group'" class="group-section">
        <div class="section-header">
          <h2>酒店团购</h2>
          <a href="#" class="view-all">查看全部</a>
        </div>
        <div class="group-list">
          <div class="group-item" v-for="(group, index) in groupDeals" :key="index" @click="viewGroupDetail(group.id)">
            <div class="group-image">
              <img :src="group.imageUrl" alt="Group deal image">
              <div class="discount-tag">{{ group.discount }}折</div>
            </div>
            <div class="group-info">
              <h3>{{ group.name }}</h3>
              <p class="group-description">{{ group.description }}</p>
              <div class="price">
                <span class="current-price">¥{{ group.currentPrice }}</span>
                <span class="original-price">¥{{ group.originalPrice }}</span>
              </div>
              <div class="sales-count">已售{{ group.salesCount }}份</div>
            </div>
          </div>
        </div>
      </section>

      <!-- 酒店榜 -->
      <section v-if="activeSection === 'ranking'" class="ranking-section">
        <div class="section-header">
          <h2>酒店榜</h2>
          <a href="#" class="view-all">查看全部</a>
        </div>
        <div class="ranking-list">
          <div class="ranking-item" v-for="(hotel, index) in rankedHotels" :key="index" @click="viewHotelDetail(hotel.id)">
            <div class="ranking-number">{{ index + 1 }}</div>
            <div class="hotel-image">
              <img :src="hotel.imageUrl" alt="Hotel image">
            </div>
            <div class="hotel-info">
              <h3>{{ hotel.name }}</h3>
              <div class="hotel-meta">
                <span class="rating">{{ hotel.rating }}</span>
                <span class="price-range">{{ hotel.priceRange }}</span>
              </div>
              <div class="ranking-description">{{ hotel.rankingDescription }}</div>
            </div>
          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
// Mock data for the hotel page
const mockData = {
  currentLocation: '上海',
  categories: [
    { id: 'all', name: '全部' },
    { id: 'hotel', name: '酒店' },
    { id: 'bnb', name: '民宿' },
    { id: 'hourly', name: '钟点房' },
    { id: 'shortRent', name: '短租' }
  ],
  sections: [
    { id: 'cheap', name: '真便宜' },
    { id: 'group', name: '酒店团购' },
    { id: 'ranking', name: '酒店榜' }
  ],
  cheapHotels: [
    { 
      id: 1, 
      name: '上海外滩华尔道夫酒店', 
      imageUrl: 'https://via.placeholder.com/200x150', 
      tag: '特惠', 
      rating: '4.9', 
      distance: '2.3公里', 
      priceRange: '¥1200-2500', 
      currentPrice: 1199, 
      originalPrice: 1599
    },
    { 
      id: 2, 
      name: '上海迪士尼乐园酒店', 
      imageUrl: 'https://via.placeholder.com/200x150', 
      tag: '特惠', 
      rating: '4.8', 
      distance: '8.7公里', 
      priceRange: '¥1800-3500', 
      currentPrice: 1799, 
      originalPrice: 2199
    },
    { 
      id: 3, 
      name: '上海和平饭店', 
      imageUrl: 'https://via.placeholder.com/200x150', 
      tag: '特惠', 
      rating: '4.9', 
      distance: '1.8公里', 
      priceRange: '¥1500-2800', 
      currentPrice: 1499, 
      originalPrice: 1899
    }
  ],
  groupDeals: [
    { 
      id: 1, 
      name: '上海世茂深坑洲际酒店2天1晚套餐', 
      imageUrl: 'https://via.placeholder.com/200x150', 
      discount: '7', 
      description: '含早餐+深坑秘境门票+下午茶', 
      currentPrice: 1299, 
      originalPrice: 1899, 
      salesCount: 128
    },
    { 
      id: 2, 
      name: '上海外滩W酒店3天2晚套餐', 
      imageUrl: 'https://via.placeholder.com/200x150', 
      discount: '6.5', 
      description: '含早餐+双人晚餐+SPA券', 
      currentPrice: 2599, 
      originalPrice: 3999, 
      salesCount: 85
    }
  ],
  rankedHotels: [
    { 
      id: 1, 
      name: '上海外滩华尔道夫酒店', 
      imageUrl: 'https://via.placeholder.com/200x150', 
      rating: '4.9', 
      priceRange: '¥1200-2500', 
      rankingDescription: '外滩区域销量冠军'
    },
    { 
      id: 2, 
      name: '上海迪士尼乐园酒店', 
      imageUrl: 'https://via.placeholder.com/200x150', 
      rating: '4.8', 
      priceRange: '¥1800-3500', 
      rankingDescription: '亲子家庭首选'
    },
    { 
      id: 3, 
      name: '上海和平饭店', 
      imageUrl: 'https://via.placeholder.com/200x150', 
      rating: '4.9', 
      priceRange: '¥1500-2800', 
      rankingDescription: '历史文化地标'
    }
  ]
}

export default {
  data() {
    return {
      ...mockData,
      searchKeyword: '',
      checkInDate: '',
      checkOutDate: '',
      activeCategory: 'all',
      activeSection: 'cheap'
    }
  },
  methods: {
    selectLocation() {
      // TODO: Implement location selection
      wx.showToast({ title: '选择位置', icon: 'none' })
    },
    showSearchModal() {
      // TODO: Implement search modal
      wx.showToast({ title: '搜索', icon: 'none' })
    },
    onSearchInput() {
      // TODO: Implement search functionality
    },
    selectCheckInDate() {
      // TODO: Implement check-in date selection
      wx.showToast({ title: '选择入住日期', icon: 'none' })
    },
    selectCheckOutDate() {
      // TODO: Implement check-out date selection
      wx.showToast({ title: '选择离店日期', icon: 'none' })
    },
    selectCategory(categoryId) {
      this.activeCategory = categoryId
      // TODO: Filter hotels based on category
    },
    viewHotelDetail(hotelId) {
      wx.navigateTo({ url: '/pages/hotelDetail/main?id=' + hotelId })
    },
    viewGroupDetail(groupId) {
      wx.navigateTo({ url: '/pages/groupDetail/main?id=' + groupId })
    }
  },
  mounted() {
    // Initialize page data
  }
}
</script>

<style lang="scss" scoped>
.hotel-container {
  background-color: $page-bgcolor;
  min-height: 100vh;
}

/* Search Bar */
.search-bar {
  display: flex;
  align-items: center;
  background-color: $nav-bgcolor;
  padding: 10rpx 20rpx;
  box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.1);

  .location-selector {
    display: flex;
    align-items: center;
    margin-right: 20rpx;
    
    .icon {
      font-size: 30rpx;
      margin-right: 5rpx;
    }
    
    span {
      font-size: $text-font * 2rpx;
      color: $textBlack-color;
    }
  }

  .search-input-wrapper {
    flex: 1;
    display: flex;
    align-items: center;
    background-color: $page-bgcolor;
    border-radius: 50rpx;
    padding: 10rpx 20rpx;

    .icon {
      font-size: $text-font * 2rpx;
      margin-right: 10rpx;
      color: $textDarkGray-color;
    }

    input {
      flex: 1;
      border: none;
      background-color: transparent;
      font-size: $text-font * 2rpx;
      outline: none;
      color: $textBlack-color;
    }
    
    input::placeholder {
      color: $placeholder-textcolor;
      font-size: $placeholder-font * 2rpx;
    }
  }
}

/* Date Range Selector */
.date-range-selector {
  display: flex;
  justify-content: space-around;
  background-color: $nav-bgcolor;
  padding: 15rpx 0;
  box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.1);

  .check-in-date, .check-out-date {
    display: flex;
    align-items: center;
    font-size: $text-font * 2rpx;
    color: $textBlack-color;

    .icon {
      margin: 0 10rpx;
      font-size: $text-font * 2rpx;
      color: $textDarkGray-color;
    }
  }

  .divider {
    align-self: center;
    font-size: $text-font * 2rpx;
    color: $textGray-color;
  }
}

/* Category Tabs */
.category-tabs {
  display: flex;
  background-color: $nav-bgcolor;
  margin-top: 10rpx;
  box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.1);
  overflow-x: auto;

  .tab-item {
    flex: 1;
    text-align: center;
    padding: 15rpx 0;
    font-size: $text-font * 2rpx;
    color: $textDarkGray-color;
    white-space: nowrap;
    
    &.active {
      color: $mtRed-color;
      border-bottom: $border-width solid $mtRed-color;
    }
  }
}

/* Main Content */
.main-content {
  .section-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 20rpx;
    background-color: $nav-bgcolor;
    border-bottom: $line-width solid $spLine-color;
    
    h2 {
      font-size: $text-font * 2.2rpx;
      font-weight: bold;
      color: $textBlack-color;
    }
    
    .view-all {
      font-size: $text-font * 2rpx;
      color: $textDarkGray-color;
    }
  }

  /* Cheap Section */
  .cheap-section {
    background-color: $nav-bgcolor;
    margin-bottom: 10rpx;
    
    .hotel-list {
      .hotel-item {
        display: flex;
        padding: 20rpx;
        border-bottom: $line-width solid $spLine-color;

        .hotel-image {
          width: 200rpx;
          height: 150rpx;
          position: relative;
          
          img {
            width: 100%;
            height: 100%;
            border-radius: 10rpx;
          }
          
          .tag {
            position: absolute;
            top: 10rpx;
            left: 10rpx;
            background-color: $mtRed-color;
            color: white;
            font-size: $btnFont-small * 2rpx;
            padding: 5rpx 10rpx;
            border-radius: 5rpx;
          }
        }
        
        .hotel-info {
          flex: 1;
          margin-left: 20rpx;
          
          h3 {
            font-size: $text-font * 2.2rpx;
            color: $textBlack-color;
            margin-bottom: 10rpx;
          }
          
          .hotel-meta {
            display: flex;
            align-items: center;
            font-size: $text-font * 1.8rpx;
            color: $textDarkGray-color;
            margin-bottom: 10rpx;
            
            span {
              margin-right: 20rpx;
            }
          }
          
          .price {
            display: flex;
            align-items: center;
            
            .current-price {
              font-size: $text-font * 2.5rpx;
              color: $mtRed-color;
              font-weight: bold;
              margin-right: 10rpx;
            }
            
            .original-price {
              font-size: $text-font * 1.8rpx;
              color: $textGray-color;
              text-decoration: line-through;
            }
          }
        }
      }
    }
  }

  /* Group Section */
  .group-section {
    background-color: $nav-bgcolor;
    margin-bottom: 10rpx;
    
    .group-list {
      .group-item {
        display: flex;
        padding: 20rpx;
        border-bottom: $line-width solid $spLine-color;

        .group-image {
          width: 200rpx;
          height: 150rpx;
          position: relative;
          
          img {
            width: 100%;
            height: 100%;
            border-radius: 10rpx;
          }
          
          .discount-tag {
            position: absolute;
            top: 0;
            right: 0;
            background-color: $mtRed-color;
            color: white;
            font-size: $btnFont-small * 2rpx;
            padding: 5rpx 10rpx;
            border-radius: 0 10rpx 0 10rpx;
          }
        }
        
        .group-info {
          flex: 1;
          margin-left: 20rpx;
          
          h3 {
            font-size: $text-font * 2.2rpx;
            color: $textBlack-color;
            margin-bottom: 10rpx;
          }
          
          .group-description {
            font-size: $text-font * 1.8rpx;
            color: $textDarkGray-color;
            margin-bottom: 10rpx;
            line-height: 1.5;
          }
          
          .price {
            display: flex;
            align-items: center;
            margin-bottom: 5rpx;
            
            .current-price {
              font-size: $text-font * 2.5rpx;
              color: $mtRed-color;
              font-weight: bold;
              margin-right: 10rpx;
            }
            
            .original-price {
              font-size: $text-font * 1.8rpx;
              color: $textGray-color;
              text-decoration: line-through;
            }
          }
          
          .sales-count {
            font-size: $text-font * 1.6rpx;
            color: $textGray-color;
          }
        }
      }
    }
  }

  /* Ranking Section */
  .ranking-section {
    background-color: $nav-bgcolor;
    margin-bottom: 10rpx;
    
    .ranking-list {
      .ranking-item {
        display: flex;
        padding: 20rpx;
        border-bottom: $line-width solid $spLine-color;

        .ranking-number {
          width: 40rpx;
          height: 40rpx;
          background-color: $mtRed-color;
          color: white;
          font-size: $text-font * 2rpx;
          font-weight: bold;
          display: flex;
          align-items: center;
          justify-content: center;
          border-radius: 50%;
          margin-right: 20rpx;
        }
        
        .hotel-image {
          width: 200rpx;
          height: 150rpx;
          
          img {
            width: 100%;
            height: 100%;
            border-radius: 10rpx;
          }
        }
        
        .hotel-info {
          flex: 1;
          margin-left: 20rpx;
          
          h3 {
            font-size: $text-font * 2.2rpx;
            color: $textBlack-color;
            margin-bottom: 10rpx;
          }
          
          .hotel-meta {
            display: flex;
            align-items: center;
            font-size: $text-font * 1.8rpx;
            color: $textDarkGray-color;
            margin-bottom: 10rpx;
            
            span {
              margin-right: 20rpx;
            }
          }
          
          .ranking-description {
            font-size: $text-font * 1.8rpx;
            color: $textDarkGray-color;
          }
        }
      }
    }
  }
}
</style>