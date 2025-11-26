<template>
  <div class="container">
    <div class="content">
      <!-- Header -->
      <div class="header-c">
        <div class="header-l" @click="addressClick">
          <i class="icon mt-location-o" :style="{color: '#434343', 'font-size': 38 + 'rpx'}"></i>
          <span>上海市漕河泾开发区</span>
          <i class="icon mt-arrow-right-o" :style="{color: '#434343', 'font-size': 28 + 'rpx'}"></i>
        </div>
        <div class="header-r" @click="searchClick">
          <i class="icon mt-search-o"></i>
          <span>搜索美食</span>
        </div>
      </div>
      
      <!-- Category Tabs -->
      <div class="category-tabs">
        <div class="tab-item" v-for="(item, index) in categoryTabs" :key="index" 
             :class="{active: activeTab === index}" @click="switchTab(index)">
          {{item.title}}
        </div>
      </div>
      
      <!-- Limited Time Flash Sale -->
      <div class="tab-content" v-if="activeTab === 0">
        <div class="flash-sale-section">
          <div class="section-header">
            <span class="title">限时秒杀</span>
            <span class="countdown">剩余时间: {{countdownTime}}</span>
          </div>
          <div class="flash-sale-list">
            <div class="flash-sale-item" v-for="(item, index) in flashSaleList" :key="index">
              <div class="item-img">
                <img :src="item.image">
              </div>
              <div class="item-info">
                <div class="item-name">{{item.name}}</div>
                <div class="price-row">
                  <span class="price">{{item.price}}</span>
                  <span class="original-price">{{item.originalPrice}}</span>
                  <span class="discount">{{item.discount}}</span>
                </div>
                <div class="progress-bar">
                  <div class="progress" :style="{width: item.soldPercentage + '%'}"></div>
                </div>
                <div class="sold-info">已售{{item.sold}}</div>
              </div>
            </div>
          </div>
        </div>
      </div>
      
      <!-- Snack and Fast Food Section -->
      <div class="tab-content" v-if="activeTab === 1">
        <div class="snack-section">
          <div class="section-header">
            <span class="title">热门小吃</span>
          </div>
          <div class="category-grid">
            <div class="category-item" v-for="(item, index) in snackCategories" :key="index">
              <img :src="item.image" class="category-img">
              <span class="category-name">{{item.name}}</span>
            </div>
          </div>
          <div class="recommended-list">
            <div class="recommended-item" v-for="(item, index) in snackList" :key="index">
              <div class="item-img">
                <img :src="item.image">
                <div class="tag" v-if="item.tag">{{item.tag}}</div>
              </div>
              <div class="item-info">
                <div class="item-name">{{item.name}}</div>
                <div class="rating-row">
                  <div class="rating">
                    <i class="icon mt-star-s" v-for="(star, idx) in Math.floor(item.rating)" :key="idx"></i>
                    <i class="icon mt-star-half-o" v-if="item.rating % 1 !== 0"></i>
                    <span>{{item.rating}}</span>
                  </div>
                  <span class="sales">月售{{item.sales}}</span>
                  <span class="distance">{{item.distance}}</span>
                </div>
                <div class="price-row">
                  <span class="price">¥{{item.price}}</span>
                  <span class="original-price" v-if="item.originalPrice">¥{{item.originalPrice}}</span>
                </div>
                <div class="tags">
                  <span class="tag" v-for="(tag, idx) in item.tags" :key="idx">{{tag}}</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      
      <!-- Food Ranking Section -->
      <div class="tab-content" v-if="activeTab === 2">
        <div class="ranking-section">
          <div class="section-header">
            <span class="title">美食排行</span>
            <div class="ranking-type">
              <div class="type-item" v-for="(item, index) in rankingTypes" :key="index" 
                   :class="{active: activeRankingType === index}" @click="switchRankingType(index)">
                {{item.title}}
              </div>
            </div>
          </div>
          <div class="ranking-list">
            <div class="ranking-item" v-for="(item, index) in rankingList" :key="index">
              <div class="ranking-number">{{index + 1}}</div>
              <div class="item-img">
                <img :src="item.image">
              </div>
              <div class="item-info">
                <div class="item-name">{{item.name}}</div>
                <div class="rating-row">
                  <div class="rating">
                    <i class="icon mt-star-s" v-for="(star, idx) in Math.floor(item.rating)" :key="idx"></i>
                    <i class="icon mt-star-half-o" v-if="item.rating % 1 !== 0"></i>
                    <span>{{item.rating}}</span>
                  </div>
                  <span class="sales">月售{{item.sales}}</span>
                </div>
                <div class="price-row">
                  <span class="price">¥{{item.price}}</span>
                </div>
                <div class="tags">
                  <span class="tag" v-for="(tag, idx) in item.tags" :key="idx">{{tag}}</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      
      <!-- Student Zone Section -->
      <div class="tab-content" v-if="activeTab === 3">
        <div class="student-section">
          <div class="section-header">
            <span class="title">学生专区</span>
            <div class="student-tag">学生专享</div>
          </div>
          <div class="student-banner">
            <img src="https://via.placeholder.com/750x200" alt="Student Banner">
          </div>
          <div class="discount-list">
            <div class="discount-item" v-for="(item, index) in studentDiscountList" :key="index">
              <div class="discount-img">
                <img :src="item.image">
              </div>
              <div class="discount-info">
                <div class="discount-title">{{item.title}}</div>
                <div class="discount-desc">{{item.description}}</div>
                <button class="get-btn" @click="getDiscount">立即领取</button>
              </div>
            </div>
          </div>
          <div class="recommended-list">
            <div class="recommended-item" v-for="(item, index) in studentRecommendList" :key="index">
              <div class="item-img">
                <img :src="item.image">
                <div class="tag" v-if="item.tag">{{item.tag}}</div>
              </div>
              <div class="item-info">
                <div class="item-name">{{item.name}}</div>
                <div class="rating-row">
                  <div class="rating">
                    <i class="icon mt-star-s" v-for="(star, idx) in Math.floor(item.rating)" :key="idx"></i>
                    <i class="icon mt-star-half-o" v-if="item.rating % 1 !== 0"></i>
                    <span>{{item.rating}}</span>
                  </div>
                  <span class="sales">月售{{item.sales}}</span>
                  <span class="distance">{{item.distance}}</span>
                </div>
                <div class="price-row">
                  <span class="price">¥{{item.price}}</span>
                  <span class="original-price" v-if="item.originalPrice">¥{{item.originalPrice}}</span>
                  <span class="student-price" v-if="item.studentPrice">学生价¥{{item.studentPrice}}</span>
                </div>
                <div class="tags">
                  <span class="tag" v-for="(tag, idx) in item.tags" :key="idx">{{tag}}</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      
      <!-- Recommended Products -->
      <div class="recommended-section">
        <div class="section-header">
          <span class="title">推荐商品</span>
          <div class="filter-bar">
            <div class="filter-item" v-for="(item, index) in filterOptions" :key="index" 
                 :class="{active: activeFilter === index}" @click="switchFilter(index)">
              {{item.title}}
            </div>
          </div>
        </div>
        <div class="recommended-list">
          <div class="recommended-item" v-for="(item, index) in recommendedList" :key="index">
            <div class="item-img">
              <img :src="item.image">
              <div class="tag" v-if="item.tag">{{item.tag}}</div>
            </div>
            <div class="item-info">
              <div class="item-name">{{item.name}}</div>
              <div class="rating-row">
                <div class="rating">
                  <i class="icon mt-star-s" v-for="(star, idx) in Math.floor(item.rating)" :key="idx"></i>
                  <i class="icon mt-star-half-o" v-if="item.rating % 1 !== 0"></i>
                  <span>{{item.rating}}</span>
                </div>
                <span class="sales">月售{{item.sales}}</span>
                <span class="distance">{{item.distance}}</span>
              </div>
              <div class="price-row">
                <span class="price">¥{{item.price}}</span>
                <span class="original-price" v-if="item.originalPrice">¥{{item.originalPrice}}</span>
              </div>
              <div class="tags">
                <span class="tag" v-for="(tag, idx) in item.tags" :key="idx">{{tag}}</span>
              </div>
            </div>
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
      activeTab: 0,
      activeFilter: 0,
      activeRankingType: 0,
      categoryTabs: [
        {title: '奶茶咖啡'},
        {title: '小吃快餐'},
        {title: '美食排行'},
        {title: '学生专区'}
      ],
      filterOptions: [
        {title: '综合排序'},
        {title: '距离最近'},
        {title: '评分最高'},
        {title: '价格最低'}
      ],
      rankingTypes: [
        {title: '热销榜'},
        {title: '好评榜'},
        {title: '新品榜'}
      ],
      countdownTime: '2:59:45', // Mock countdown time
      flashSaleList: [
        {image: 'https://via.placeholder.com/200x200', name: '珍珠奶茶', price: '¥9.9', originalPrice: '¥18', discount: '5.5折', soldPercentage: 75, sold: '321'},
        {image: 'https://via.placeholder.com/200x200', name: '炸鸡套餐', price: '¥19.9', originalPrice: '¥35', discount: '5.7折', soldPercentage: 50, sold: '189'},
        {image: 'https://via.placeholder.com/200x200', name: '水果沙拉', price: '¥12.9', originalPrice: '¥22', discount: '5.9折', soldPercentage: 90, sold: '456'}
      ],
      snackCategories: [
        {image: 'https://via.placeholder.com/100x100', name: '炸鸡'},
        {image: 'https://via.placeholder.com/100x100', name: '汉堡'},
        {image: 'https://via.placeholder.com/100x100', name: '披萨'},
        {image: 'https://via.placeholder.com/100x100', name: '麻辣烫'},
        {image: 'https://via.placeholder.com/100x100', name: '寿司'},
        {image: 'https://via.placeholder.com/100x100', name: '便当'}
      ],
      snackList: [
        {image: 'https://via.placeholder.com/200x200', name: '肯德基香辣鸡翅', rating: 4.8, sales: '2345', distance: '1.2km', price: '29.9', originalPrice: '39.9', tags: ['满30减5', '免配送费'], tag: '热销'},
        {image: 'https://via.placeholder.com/200x200', name: '麦当劳麦乐鸡', rating: 4.7, sales: '1890', distance: '0.8km', price: '19.9', originalPrice: '25.9', tags: ['第二份半价']},
        {image: 'https://via.placeholder.com/200x200', name: '正新鸡排', rating: 4.6, sales: '1567', distance: '1.5km', price: '15.9', originalPrice: '19.9', tags: ['满20减8', '新品'], tag: '新品'}
      ],
      rankingList: [
        {image: 'https://via.placeholder.com/200x200', name: '星巴克拿铁咖啡', rating: 4.8, sales: '1234', price: '35', tags: ['热销', '好评']},
        {image: 'https://via.placeholder.com/200x200', name: '肯德基全家桶', rating: 4.6, sales: '987', price: '89', tags: ['热销', '套餐']},
        {image: 'https://via.placeholder.com/200x200', name: '麦当劳麦辣鸡腿堡', rating: 4.7, sales: '1567', price: '22', tags: ['热销', '单品']}
      ],
      studentDiscountList: [
        {image: 'https://via.placeholder.com/150x100', title: '学生专享满20减10', description: '限每日10:00-14:00使用'},
        {image: 'https://via.placeholder.com/150x100', title: '学生专享第二份半价', description: '限奶茶咖啡类产品'},
        {image: 'https://via.placeholder.com/150x100', title: '学生专享免配送费', description: '满30元免配送费'}
      ],
      studentRecommendList: [
        {image: 'https://via.placeholder.com/200x200', name: 'COCO奶茶', rating: 4.5, sales: '2345', distance: '1.5km', price: '15', originalPrice: '20', studentPrice: '12', tags: ['满20减8', '学生专享'], tag: '新品'},
        {image: 'https://via.placeholder.com/200x200', name: '麦当劳学生套餐', rating: 4.6, sales: '1234', distance: '0.8km', price: '29.9', originalPrice: '39.9', studentPrice: '25', tags: ['学生专享', '套餐'], tag: '热销'},
        {image: 'https://via.placeholder.com/200x200', name: '正新鸡排学生餐', rating: 4.4, sales: '987', distance: '1.2km', price: '18.9', originalPrice: '23.9', studentPrice: '15', tags: ['学生专享', '小吃'], tag: '热销'}
      ],
      recommendedList: [
        {image: 'https://via.placeholder.com/200x200', name: '星巴克拿铁咖啡', rating: 4.8, sales: '1234', distance: '1.2km', price: '35', originalPrice: '42', tags: ['满30减5', '免配送费'], tag: '热销'},
        {image: 'https://via.placeholder.com/200x200', name: '肯德基全家桶', rating: 4.6, sales: '987', distance: '2.5km', price: '89', originalPrice: '128', tags: ['满100减20', '会员专享']},
        {image: 'https://via.placeholder.com/200x200', name: '麦当劳麦辣鸡腿堡', rating: 4.7, sales: '1567', distance: '0.8km', price: '22', originalPrice: '28', tags: ['第二份半价']},
        {image: 'https://via.placeholder.com/200x200', name: 'COCO奶茶', rating: 4.5, sales: '2345', distance: '1.5km', price: '15', originalPrice: '20', tags: ['满20减8', '新品'], tag: '新品'}
      ]
    }
  },
  methods: {
    switchTab(index) {
      this.activeTab = index;
    },
    switchFilter(index) {
      this.activeFilter = index;
      // Simulate sorting based on filter option
      if (index === 1) { // Distance
        this.recommendedList.sort((a, b) => parseFloat(a.distance) - parseFloat(b.distance));
      } else if (index === 2) { // Rating
        this.recommendedList.sort((a, b) => b.rating - a.rating);
      } else if (index === 3) { // Price
        this.recommendedList.sort((a, b) => parseFloat(a.price) - parseFloat(b.price));
      } else { // Default
        // Reset to original order
        this.recommendedList = [
          {image: 'https://via.placeholder.com/200x200', name: '星巴克拿铁咖啡', rating: 4.8, sales: '1234', distance: '1.2km', price: '35', originalPrice: '42', tags: ['满30减5', '免配送费'], tag: '热销'},
          {image: 'https://via.placeholder.com/200x200', name: '肯德基全家桶', rating: 4.6, sales: '987', distance: '2.5km', price: '89', originalPrice: '128', tags: ['满100减20', '会员专享']},
          {image: 'https://via.placeholder.com/200x200', name: '麦当劳麦辣鸡腿堡', rating: 4.7, sales: '1567', distance: '0.8km', price: '22', originalPrice: '28', tags: ['第二份半价']},
          {image: 'https://via.placeholder.com/200x200', name: 'COCO奶茶', rating: 4.5, sales: '2345', distance: '1.5km', price: '15', originalPrice: '20', tags: ['满20减8', '新品'], tag: '新品'}
        ];
      }
    },
    switchRankingType(index) {
      this.activeRankingType = index;
      // Simulate ranking change
      if (index === 1) { // Good Reviews
        this.rankingList.sort((a, b) => b.rating - a.rating);
      } else if (index === 2) { // New Products
        // For demo, just reverse the list
        this.rankingList.reverse();
      } else { // Default
        // Reset to original order
        this.rankingList = [
          {image: 'https://via.placeholder.com/200x200', name: '星巴克拿铁咖啡', rating: 4.8, sales: '1234', price: '35', tags: ['热销', '好评']},
          {image: 'https://via.placeholder.com/200x200', name: '肯德基全家桶', rating: 4.6, sales: '987', price: '89', tags: ['热销', '套餐']},
          {image: 'https://via.placeholder.com/200x200', name: '麦当劳麦辣鸡腿堡', rating: 4.7, sales: '1567', price: '22', tags: ['热销', '单品']}
        ];
      }
    },
    getDiscount() {
      // Handle discount click
      console.log('领取优惠券');
    },
    addressClick() {
      // Handle address click
    },
    searchClick() {
      // Handle search click
    }
  },
  mounted() {
    // Simulate countdown timer
    setInterval(() => {
      const timeArr = this.countdownTime.split(':');
      let hours = parseInt(timeArr[0]);
      let minutes = parseInt(timeArr[1]);
      let seconds = parseInt(timeArr[2]);
      
      seconds--;
      if (seconds < 0) {
        seconds = 59;
        minutes--;
        if (minutes < 0) {
          minutes = 59;
          hours--;
          if (hours < 0) {
            hours = 0;
            minutes = 0;
            seconds = 0;
          }
        }
      }
      
      this.countdownTime = `${hours.toString().padStart(2, '0')}:${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
    }, 1000);
  }
}
</script>

<style scoped>
.container {
  background-color: #f5f5f5;
  min-height: 100vh;
}

.content {
  padding-bottom: 120rpx;
}

/* Header */
.header-c {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20rpx 30rpx;
  background-color: #fff;
}

.header-l {
  display: flex;
  align-items: center;
}

.header-l span {
  margin: 0 10rpx;
  font-size: 32rpx;
}

.header-r {
  display: flex;
  align-items: center;
  background-color: #f5f5f5;
  padding: 15rpx 20rpx;
  border-radius: 20rpx;
}

.header-r span {
  margin-left: 10rpx;
  font-size: 28rpx;
  color: #999;
}

/* Category Tabs */
.category-tabs {
  display: flex;
  background-color: #fff;
  border-bottom: 1rpx solid #eee;
}

.tab-item {
  flex: 1;
  text-align: center;
  padding: 30rpx 0;
  font-size: 32rpx;
  color: #666;
}

.tab-item.active {
  color: #FFC24A;
  border-bottom: 4rpx solid #FFC24A;
}

/* Section Header */
.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 30rpx;
}

.section-header .title {
  font-size: 36rpx;
  font-weight: bold;
}

.section-header .countdown {
  font-size: 28rpx;
  color: #f60;
}

/* Filter Bar */
.filter-bar {
  display: flex;
}

.filter-item {
  margin-left: 30rpx;
  padding: 10rpx 20rpx;
  font-size: 28rpx;
  color: #666;
}

.filter-item.active {
  background-color: #FFC24A;
  color: #fff;
  border-radius: 20rpx;
}

/* Flash Sale Section */
.flash-sale-section {
  background-color: #fff;
  margin-top: 20rpx;
}

.flash-sale-list {
  display: flex;
  overflow-x: auto;
  padding: 0 30rpx 30rpx;
}

.flash-sale-item {
  width: 280rpx;
  margin-right: 20rpx;
}

.flash-sale-item .item-img {
  position: relative;
}

.flash-sale-item .item-img img {
  width: 100%;
  height: 280rpx;
  border-radius: 10rpx;
}

.flash-sale-item .item-info {
  margin-top: 10rpx;
}

.flash-sale-item .item-name {
  font-size: 28rpx;
  line-height: 1.4;
  height: 80rpx;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}

.flash-sale-item .price-row {
  display: flex;
  align-items: baseline;
  margin-top: 10rpx;
}

.flash-sale-item .price {
  font-size: 32rpx;
  font-weight: bold;
  color: #f60;
  margin-right: 10rpx;
}

.flash-sale-item .original-price {
  font-size: 24rpx;
  color: #999;
  text-decoration: line-through;
  margin-right: 10rpx;
}

.flash-sale-item .discount {
  font-size: 24rpx;
  color: #fff;
  background-color: #f60;
  padding: 2rpx 6rpx;
  border-radius: 4rpx;
}

.flash-sale-item .progress-bar {
  height: 10rpx;
  background-color: #eee;
  border-radius: 5rpx;
  margin-top: 10rpx;
}

.flash-sale-item .progress {
  height: 100%;
  background-color: #f60;
  border-radius: 5rpx;
}

.flash-sale-item .sold-info {
  font-size: 24rpx;
  color: #999;
  margin-top: 10rpx;
  text-align: right;
}

/* Recommended Section */
.recommended-section {
  background-color: #fff;
  margin-top: 20rpx;
}

.recommended-list {
  padding: 0 30rpx 30rpx;
}

.recommended-item {
  display: flex;
  padding: 30rpx 0;
  border-bottom: 1rpx solid #eee;
}

.recommended-item:last-child {
  border-bottom: none;
}

.recommended-item .item-img {
  width: 180rpx;
  height: 180rpx;
  position: relative;
  margin-right: 30rpx;
}

.recommended-item .item-img img {
  width: 100%;
  height: 100%;
  border-radius: 10rpx;
}

.recommended-item .item-img .tag {
  position: absolute;
  top: 10rpx;
  left: 10rpx;
  background-color: #f60;
  color: #fff;
  font-size: 24rpx;
  padding: 4rpx 8rpx;
  border-radius: 4rpx;
}

.recommended-item .item-info {
  flex: 1;
}

.recommended-item .item-name {
  font-size: 32rpx;
  line-height: 1.4;
  height: 90rpx;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}

.recommended-item .rating-row {
  display: flex;
  align-items: center;
  margin-top: 10rpx;
  font-size: 24rpx;
  color: #999;
}

.recommended-item .rating {
  display: flex;
  align-items: center;
}

.recommended-item .rating i {
  color: #FFC24A;
  margin-right: 2rpx;
}

.recommended-item .rating span {
  margin-left: 10rpx;
}

.recommended-item .sales {
  margin-left: 30rpx;
}

.recommended-item .distance {
  margin-left: 30rpx;
}

.recommended-item .price-row {
  display: flex;
  align-items: baseline;
  margin-top: 10rpx;
}

.recommended-item .price {
  font-size: 32rpx;
  font-weight: bold;
  color: #f60;
  margin-right: 10rpx;
}

.recommended-item .original-price {
  font-size: 24rpx;
  color: #999;
  text-decoration: line-through;
}

.recommended-item .tags {
  display: flex;
  flex-wrap: wrap;
  margin-top: 10rpx;
}

.recommended-item .tags .tag {
  font-size: 24rpx;
  color: #666;
  background-color: #f5f5f5;
  padding: 4rpx 8rpx;
  border-radius: 4rpx;
  margin-right: 10rpx;
  margin-bottom: 10rpx;
}
/* Snack Category Section */
.snack-category-section {
  background-color: #fff;
  padding: 30rpx;
  margin-top: 20rpx;
}

.snack-categories {
  display: flex;
  justify-content: space-between;
  margin-bottom: 30rpx;
}

.snack-category-item {
  text-align: center;
}

.snack-category-item img {
  width: 100rpx;
  height: 100rpx;
  border-radius: 50%;
}

.snack-category-item p {
  margin-top: 10rpx;
  font-size: 24rpx;
}

.snack-recommended-section {
  background-color: #fff;
  margin-top: 20rpx;
}

/* Ranking Section */
.ranking-section {
  background-color: #fff;
  margin-top: 20rpx;
}

.ranking-tabs {
  display: flex;
  background-color: #f5f5f5;
  padding: 10rpx;
  border-radius: 10rpx;
  margin: 0 30rpx 30rpx;
}

.ranking-tab-item {
  flex: 1;
  text-align: center;
  padding: 10rpx 0;
  font-size: 28rpx;
  color: #666;
  border-radius: 8rpx;
}

.ranking-tab-item.active {
  background-color: #fff;
  color: #FFC24A;
}

.ranking-list {
  padding: 0 30rpx 30rpx;
}

.ranking-item {
  display: flex;
  padding: 20rpx 0;
  border-bottom: 1rpx solid #eee;
}

.ranking-item:last-child {
  border-bottom: none;
}

.ranking-item .ranking-num {
  font-size: 36rpx;
  font-weight: bold;
  color: #FFC24A;
  margin-right: 20rpx;
  width: 60rpx;
  text-align: center;
}

.ranking-item .item-img {
  width: 120rpx;
  height: 120rpx;
  margin-right: 20rpx;
}

.ranking-item .item-img img {
  width: 100%;
  height: 100%;
  border-radius: 10rpx;
}

.ranking-item .item-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.ranking-item .item-name {
  font-size: 28rpx;
  line-height: 1.4;
}

.ranking-item .rating {
  font-size: 24rpx;
  color: #999;
  margin-top: 5rpx;
}

.ranking-item .sales {
  font-size: 24rpx;
  color: #999;
  margin-top: 5rpx;
}

.ranking-item .price {
  font-size: 32rpx;
  font-weight: bold;
  color: #f60;
  margin-top: 5rpx;
}

/* Student Zone Section */
.student-zone-section {
  background-color: #fff;
  margin-top: 20rpx;
}

.student-banner {
  position: relative;
  margin: 0 30rpx 30rpx;
}

.student-banner img {
  width: 100%;
  height: 200rpx;
  border-radius: 10rpx;
}

.student-banner .banner-text {
  position: absolute;
  top: 50%;
  left: 30rpx;
  transform: translateY(-50%);
  color: #fff;
}

.student-banner .banner-text .title {
  font-size: 36rpx;
  font-weight: bold;
}

.student-banner .banner-text .sub-title {
  font-size: 28rpx;
  margin-top: 10rpx;
}

.student-discounts {
  padding: 0 30rpx 30rpx;
}

.discount-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20rpx;
  background-color: #f9f9f9;
  border-radius: 10rpx;
  margin-bottom: 20rpx;
}

.discount-item .discount-info {
  display: flex;
  align-items: center;
}

.discount-item .discount-amount {
  font-size: 36rpx;
  font-weight: bold;
  color: #f60;
  margin-right: 10rpx;
}

.discount-item .discount-desc {
  font-size: 28rpx;
}

.discount-item .discount-btn {
  background-color: #FFC24A;
  color: #fff;
  padding: 10rpx 20rpx;
  border-radius: 20rpx;
  font-size: 24rpx;
}

.student-recommended-section {
  background-color: #fff;
  margin-top: 20rpx;
}

.student-recommended-section .recommended-list {
  padding: 0 30rpx 30rpx;
}

.student-recommended-section .recommended-item {
  display: flex;
  padding: 20rpx 0;
  border-bottom: 1rpx solid #eee;
}

.student-recommended-section .recommended-item:last-child {
  border-bottom: none;
}

.student-recommended-section .recommended-item .item-img {
  width: 150rpx;
  height: 150rpx;
  margin-right: 20rpx;
}

.student-recommended-section .recommended-item .item-img img {
  width: 100%;
  height: 100%;
  border-radius: 10rpx;
}

.student-recommended-section .recommended-item .item-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.student-recommended-section .recommended-item .item-name {
  font-size: 28rpx;
  line-height: 1.4;
}

.student-recommended-section .recommended-item .price {
  font-size: 32rpx;
  font-weight: bold;
  color: #f60;
  margin-top: 10rpx;
}

.student-recommended-section .recommended-item .student-tag {
  font-size: 24rpx;
  color: #f60;
  margin-top: 5rpx;
}

</style>