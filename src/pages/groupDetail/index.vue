<template>
  <div class="group-detail-container">
    <!-- Group Image -->
    <div class="group-image">
      <img :src="groupDeal.image" alt="Group deal image">
      <div class="discount-tag">{{ groupDeal.discount }}折</div>
    </div>

    <!-- Group Basic Info -->
    <div class="group-basic-info">
      <h1 class="group-name">{{ groupDeal.name }}</h1>
      <div class="group-price">
        <span class="current-price">¥{{ groupDeal.currentPrice }}</span>
        <span class="original-price">¥{{ groupDeal.originalPrice }}</span>
      </div>
      <div class="group-meta">
        <span class="sales-count">已售{{ groupDeal.salesCount }}份</span>
        <span class="validity">有效期至{{ groupDeal.validity }}</span>
        <span class="book-count">今日已订{{ groupDeal.bookCount }}份</span>
      </div>
    </div>

    <!-- Group Description -->
    <div class="group-description-section">
      <div class="section-header">
        <h2>套餐包含</h2>
      </div>
      <div class="description-content">
        <p>{{ groupDeal.description }}</p>
        <ul>
          <li v-for="(item, index) in groupDeal.inclusions" :key="index">{{ item }}</li>
        </ul>
      </div>
    </div>

    <!-- Group Details -->
    <div class="group-details-section">
      <div class="section-header">
        <h2>使用须知</h2>
      </div>
      <div class="details-content">
        <div class="detail-item">
          <span class="detail-label">预订须知：</span>
          <span class="detail-value">{{ groupDeal.reservationPolicy }}</span>
        </div>
        <div class="detail-item">
          <span class="detail-label">使用时间：</span>
          <span class="detail-value">{{ groupDeal.usageTime }}</span>
        </div>
        <div class="detail-item">
          <span class="detail-label">使用地点：</span>
          <span class="detail-value">{{ groupDeal.location }}</span>
        </div>
        <div class="detail-item">
          <span class="detail-label">购买须知：</span>
          <span class="detail-value">{{ groupDeal.purchasePolicy }}</span>
        </div>
      </div>
    </div>

    <!-- Hotel Info -->
    <div class="hotel-info-section">
      <div class="section-header">
        <h2>酒店信息</h2>
      </div>
      <div class="hotel-info-content">
        <div class="hotel-basic">
          <img class="hotel-image" :src="groupDeal.hotel.image" alt="Hotel image">
          <div class="hotel-details">
            <h3 class="hotel-name">{{ groupDeal.hotel.name }}</h3>
            <p class="hotel-location">{{ groupDeal.hotel.location }}</p>
            <div class="hotel-rating">
              <span class="rating-score">{{ groupDeal.hotel.rating }}</span>
              <span class="rating-count">({{ groupDeal.hotel.ratingCount }}条评价)</span>
            </div>
          </div>
        </div>
        <button class="view-hotel" @click="viewHotelDetail(groupDeal.hotel.id)">查看酒店详情</button>
      </div>
    </div>

    <!-- User Reviews -->
    <div class="reviews-section">
      <div class="section-header">
        <h2>用户评价</h2>
        <a href="#" class="view-all">查看全部</a>
      </div>
      <div class="review-list">
        <div class="review-item" v-for="(review, index) in groupDeal.reviews" :key="index">
          <div class="review-author">
            <img class="author-avatar" :src="review.avatar" alt="User avatar">
            <span class="author-name">{{ review.name }}</span>
            <span class="review-rating">{{ review.rating }}分</span>
          </div>
          <p class="review-content">{{ review.content }}</p>
          <div class="review-meta">
            <span class="review-date">{{ review.date }}</span>
          </div>
        </div>
      </div>
    </div>

    <!-- Book Now Button -->
    <div class="book-now-section">
      <button class="book-now-btn" @click="bookNow">立即购买</button>
    </div>
  </div>
</template>

<script>
// Mock data for group detail page
const mockGroupData = {
  id: 1,
  name: '上海世茂深坑洲际酒店2天1晚套餐',
  discount: '7',
  currentPrice: 1299,
  originalPrice: 1899,
  salesCount: 128,
  validity: '2023-12-31',
  bookCount: 5,
  image: 'https://via.placeholder.com/800x400',
  description: '含早餐+深坑秘境门票+下午茶',
  inclusions: [
    '上海世茂深坑洲际酒店1晚住宿',
    '次日双人早餐',
    '双人深坑秘境门票',
    '双人下午茶一份'
  ],
  reservationPolicy: '请提前3天预约，周末节假日通用',
  usageTime: '2023年6月1日-2023年12月31日',
  location: '上海市松江区辰花路5888号',
  purchasePolicy: '未使用可随时退款，过期自动退款',
  hotel: {
    id: 1,
    name: '上海世茂深坑洲际酒店',
    image: 'https://via.placeholder.com/200x150',
    location: '上海市松江区辰花路5888号',
    rating: 4.8,
    ratingCount: 189
  },
  reviews: [
    {
      avatar: 'https://via.placeholder.com/50x50',
      name: '王五',
      rating: 5,
      content: '套餐非常划算，酒店环境很棒，早餐也很丰富。',
      date: '2023-05-20'
    },
    {
      avatar: 'https://via.placeholder.com/50x50',
      name: '赵六',
      rating: 4.5,
      content: '酒店位置有点偏，但环境确实不错，套餐性价比很高。',
      date: '2023-05-15'
    }
  ]
}

export default {
  data() {
    return {
      groupDeal: mockGroupData
    }
  },
  methods: {
    viewHotelDetail(hotelId) {
      wx.navigateTo({ url: '/pages/hotelDetail/main?id=' + hotelId })
    },
    bookNow() {
      // TODO: Implement booking functionality
      wx.showToast({ title: '立即购买', icon: 'none' })
    }
  },
  mounted() {
    // Get group ID from URL parameter
    const groupId = this.$root.$mp.query.id
    // TODO: Fetch group data based on ID
  }
}
</script>

<style lang="scss" scoped>
.group-detail-container {
  background-color: $page-bgcolor;
  min-height: 100vh;
}

/* Group Image */
.group-image {
  position: relative;

  img {
    width: 100%;
    height: 400rpx;
    object-fit: cover;
  }

  .discount-tag {
    position: absolute;
    top: 20rpx;
    left: 20rpx;
    background-color: $mtRed-color;
    color: white;
    padding: 10rpx 20rpx;
    font-size: $text-font * 2rpx;
    border-radius: 5rpx;
  }
}

/* Group Basic Info */
.group-basic-info {
  background-color: $nav-bgcolor;
  padding: 20rpx;
  margin-bottom: 10rpx;

  .group-name {
    font-size: $text-font * 2.6rpx;
    font-weight: bold;
    color: $textBlack-color;
    margin-bottom: 15rpx;
  }

  .group-price {
    display: flex;
    align-items: center;
    margin-bottom: 15rpx;

    .current-price {
      font-size: $text-font * 2.6rpx;
      font-weight: bold;
      color: $mtRed-color;
      margin-right: 10rpx;
    }

    .original-price {
      font-size: $text-font * 1.7rpx;
      color: $textGray-color;
      text-decoration: line-through;
    }
  }

  .group-meta {
    display: flex;
    flex-wrap: wrap;
    font-size: $text-font * 1.7rpx;
    color: $textGray-color;

    span {
      margin-right: 20rpx;
      margin-bottom: 10rpx;
    }
  }
}

/* Group Description Section */
.group-description-section {
  background-color: $nav-bgcolor;
  margin-bottom: 10rpx;

  .section-header {
    padding: 20rpx;
    border-bottom: $line-width solid $spLine-color;

    h2 {
      font-size: $text-font * 2.2rpx;
      font-weight: bold;
      color: $textBlack-color;
    }
  }

  .description-content {
    padding: 20rpx;
    font-size: $text-font * 1.8rpx;
    color: $textDarkGray-color;
    line-height: 1.5;

    ul {
      margin-top: 15rpx;
      padding-left: 20rpx;

      li {
        margin-bottom: 10rpx;
      }
    }
  }
}

/* Group Details Section */
.group-details-section {
  background-color: $nav-bgcolor;
  margin-bottom: 10rpx;

  .section-header {
    padding: 20rpx;
    border-bottom: $line-width solid $spLine-color;

    h2 {
      font-size: $text-font * 2.2rpx;
      font-weight: bold;
      color: $textBlack-color;
    }
  }

  .details-content {
    padding: 20rpx;

    .detail-item {
      display: flex;
      margin-bottom: 15rpx;
      font-size: $text-font * 1.8rpx;
      line-height: 1.5;

      .detail-label {
        font-weight: bold;
        width: 150rpx;
        flex-shrink: 0;
        color: $textBlack-color;
      }

      .detail-value {
        color: $textDarkGray-color;
      }
    }
  }
}

/* Hotel Info Section */
.hotel-info-section {
  background-color: $nav-bgcolor;
  margin-bottom: 10rpx;

  .section-header {
    padding: 20rpx;
    border-bottom: $line-width solid $spLine-color;

    h2 {
      font-size: $text-font * 2.2rpx;
      font-weight: bold;
      color: $textBlack-color;
    }
  }

  .hotel-info-content {
    padding: 20rpx;

    .hotel-basic {
      display: flex;
      margin-bottom: 15rpx;

      .hotel-image {
        width: 200rpx;
        height: 150rpx;
        object-fit: cover;
        border-radius: 10rpx;
        margin-right: 20rpx;
      }

      .hotel-details {
        flex: 1;

        .hotel-name {
          font-size: $text-font * 2.1rpx;
          color: $textBlack-color;
          margin-bottom: 10rpx;
        }

        .hotel-location {
          font-size: $text-font * 1.7rpx;
          color: $textGray-color;
          margin-bottom: 10rpx;
        }

        .hotel-rating {
          font-size: $text-font * 1.7rpx;

          .rating-score {
            color: $mtRed-color;
            margin-right: 10rpx;
          }

          .rating-count {
            color: $textGray-color;
          }
        }
      }
    }

    .view-hotel {
      background-color: $nav-bgcolor;
      color: $mtRed-color;
      border: $line-width solid $mtRed-color;
      padding: 10rpx 20rpx;
      border-radius: 5rpx;
      font-size: $text-font * 2rpx;
      width: 100%;
    }
  }
}

/* Reviews Section */
.reviews-section {
  background-color: $nav-bgcolor;
  margin-bottom: 10rpx;

  .section-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 20rpx;
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

  .review-list {
    .review-item {
      padding: 20rpx;
      border-bottom: $line-width solid $spLine-color;

      .review-author {
        display: flex;
        align-items: center;
        margin-bottom: 15rpx;

        .author-avatar {
          width: 50rpx;
          height: 50rpx;
          border-radius: 50%;
          margin-right: 10rpx;
        }

        .author-name {
          font-size: $text-font * 1.8rpx;
          color: $textBlack-color;
          margin-right: 10rpx;
        }

        .review-rating {
          font-size: $text-font * 1.7rpx;
          color: $mtRed-color;
        }
      }

      .review-content {
        font-size: $text-font * 1.8rpx;
        color: $textBlack-color;
        margin-bottom: 10rpx;
        line-height: 1.4;
      }

      .review-meta {
        font-size: $text-font * 1.7rpx;
        color: $textGray-color;

        span {
          margin-right: 20rpx;
        }
      }
    }
  }
}

/* Book Now Section */
.book-now-section {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: $nav-bgcolor;
  padding: 20rpx;
  border-top: $line-width solid $spLine-color;

  .book-now-btn {
    width: 100%;
    background-color: $mtRed-color;
    color: white;
    border: none;
    padding: 20rpx;
    border-radius: 5rpx;
    font-size: $text-font * 2.2rpx;
    font-weight: bold;
  }
}
</style>