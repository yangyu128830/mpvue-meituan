<template>
  <div class="hotel-detail-container">
    <!-- Hotel Image Slider -->
    <swiper class="image-slider" indicator-dots="true" indicator-color="#fff" indicator-active-color="#ff3800" autoplay="true">
      <block v-for="(image, index) in hotel.images" :key="index">
        <swiper-item>
          <img class="image" :src="image" alt="Hotel image">
        </swiper-item>
      </block>
    </swiper>

    <!-- Hotel Basic Info -->
    <div class="hotel-basic-info">
      <h1 class="hotel-name">{{ hotel.name }}</h1>
      <div class="hotel-rating">
        <span class="rating-score">{{ hotel.rating }}</span>
        <span class="rating-count">({{ hotel.ratingCount }}条评价)</span>
        <span class="price-range">¥{{ hotel.minPrice }}-{{ hotel.maxPrice }}</span>
      </div>
      <div class="hotel-meta">
        <span class="location">{{ hotel.location }}</span>
        <span class="distance">{{ hotel.distance }}公里</span>
        <span class="facilities" v-if="hotel.freeParking">免费停车</span>
        <span class="facilities" v-if="hotel.freeBreakfast">免费早餐</span>
      </div>
    </div>

    <!-- Room Types Section -->
    <div class="room-types-section">
      <div class="section-header">
        <h2>房型</h2>
      </div>
      <div class="room-list">
        <div class="room-item" v-for="(room, index) in hotel.rooms" :key="index">
          <div class="room-image">
            <img :src="room.image" alt="Room image">
          </div>
          <div class="room-info">
            <h3 class="room-name">{{ room.name }}</h3>
            <p class="room-description">{{ room.description }}</p>
            <div class="room-price">
              <span class="current-price">¥{{ room.price }}</span>
              <span class="original-price" v-if="room.originalPrice">¥{{ room.originalPrice }}</span>
            </div>
            <button class="book-now" @click="bookRoom(room.id)">立即预订</button>
          </div>
        </div>
      </div>
    </div>

    <!-- Hotel Facilities -->
    <div class="facilities-section">
      <div class="section-header">
        <h2>酒店设施</h2>
      </div>
      <div class="facilities-list">
        <div class="facility-item" v-for="(facility, index) in hotel.facilities" :key="index">
          <i class="icon" :class="facility.icon"></i>
          <span>{{ facility.name }}</span>
        </div>
      </div>
    </div>

    <!-- User Reviews -->
    <div class="reviews-section">
      <div class="section-header">
        <h2>用户评价</h2>
        <a href="#" class="view-all">查看全部</a>
      </div>
      <div class="review-list">
        <div class="review-item" v-for="(review, index) in hotel.reviews" :key="index">
          <div class="review-author">
            <img class="author-avatar" :src="review.avatar" alt="User avatar">
            <span class="author-name">{{ review.name }}</span>
            <span class="review-rating">{{ review.rating }}分</span>
          </div>
          <p class="review-content">{{ review.content }}</p>
          <div class="review-meta">
            <span class="review-date">{{ review.date }}</span>
            <span class="review-room">{{ review.roomType }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// Mock data for hotel detail page
const mockHotelData = {
  id: 1,
  name: '上海外滩华尔道夫酒店',
  rating: 4.9,
  ratingCount: 256,
  minPrice: 1200,
  maxPrice: 2500,
  location: '黄浦区中山东一路2号',
  distance: 2.3,
  freeParking: true,
  freeBreakfast: true,
  images: [
    'https://via.placeholder.com/800x400',
    'https://via.placeholder.com/800x400',
    'https://via.placeholder.com/800x400'
  ],
  rooms: [
    {
      id: 1,
      name: '豪华江景房',
      description: '宽敞舒适，俯瞰黄浦江景，配备独立浴室和高速WiFi',
      image: 'https://via.placeholder.com/300x200',
      price: 1599,
      originalPrice: 1899
    },
    {
      id: 2,
      name: '行政套房',
      description: '豪华套房，配备客厅和卧室，享受行政楼层待遇',
      image: 'https://via.placeholder.com/300x200',
      price: 2299,
      originalPrice: 2599
    }
  ],
  facilities: [
    { name: '免费停车场', icon: 'mt-car-o' },
    { name: '免费早餐', icon: 'mt-coffee-o' },
    { name: '健身房', icon: 'mt-weight-o' },
    { name: '游泳池', icon: 'mt-swim-o' },
    { name: '免费WiFi', icon: 'mt-wifi-o' },
    { name: '24小时前台', icon: 'mt-clock-o' }
  ],
  reviews: [
    {
      avatar: 'https://via.placeholder.com/50x50',
      name: '张三',
      rating: 5,
      content: '酒店位置非常好，就在外滩旁边，夜景很美。房间干净整洁，服务周到。',
      date: '2023-05-15',
      roomType: '豪华江景房'
    },
    {
      avatar: 'https://via.placeholder.com/50x50',
      name: '李四',
      rating: 4.5,
      content: '酒店设施齐全，早餐种类丰富。唯一的不足是电梯有点慢。',
      date: '2023-05-10',
      roomType: '行政套房'
    }
  ]
}

export default {
  data() {
    return {
      hotel: mockHotelData
    }
  },
  methods: {
    bookRoom(roomId) {
      // TODO: Implement room booking functionality
      wx.showToast({ title: '预订房间', icon: 'none' })
    }
  },
  mounted() {
    // Get hotel ID from URL parameter
    const hotelId = this.$root.$mp.query.id
    // TODO: Fetch hotel data based on ID
  }
}
</script>

<style lang="scss" scoped>
.hotel-detail-container {
  background-color: $page-bgcolor;
  min-height: 100vh;
}

/* Image Slider */
.image-slider {
  width: 100%;
  height: 400rpx;

  .image {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
}

/* Hotel Basic Info */
.hotel-basic-info {
  background-color: $nav-bgcolor;
  padding: 20rpx;
  margin-bottom: 10rpx;

  .hotel-name {
    font-size: $text-font * 2.6rpx;
    font-weight: bold;
    color: $textBlack-color;
    margin-bottom: 15rpx;
  }

  .hotel-rating {
    display: flex;
    align-items: center;
    margin-bottom: 15rpx;

    .rating-score {
      font-size: $text-font * 2.3rpx;
      font-weight: bold;
      color: $mtRed-color;
      margin-right: 10rpx;
    }

    .rating-count {
      font-size: $text-font * 1.7rpx;
      color: $textGray-color;
      margin-right: 20rpx;
    }

    .price-range {
      font-size: $text-font * 2rpx;
      font-weight: bold;
      color: $mtRed-color;
    }
  }

  .hotel-meta {
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

/* Room Types Section */
.room-types-section {
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

  .room-list {
    .room-item {
      display: flex;
      padding: 20rpx;
      border-bottom: $line-width solid $spLine-color;

      .room-image {
        width: 300rpx;
        height: 200rpx;
        margin-right: 20rpx;

        img {
          width: 100%;
          height: 100%;
          object-fit: cover;
          border-radius: 10rpx;
        }
      }

      .room-info {
        flex: 1;

        .room-name {
          font-size: $text-font * 2.1rpx;
          color: $textBlack-color;
          margin-bottom: 10rpx;
        }

        .room-description {
          font-size: $text-font * 1.7rpx;
          color: $textDarkGray-color;
          margin-bottom: 15rpx;
          line-height: 1.4;
        }

        .room-price {
          display: flex;
          align-items: center;
          margin-bottom: 15rpx;

          .current-price {
            font-size: $text-font * 2.1rpx;
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

        .book-now {
          background-color: $mtRed-color;
          color: white;
          border: none;
          padding: 10rpx 20rpx;
          border-radius: 5rpx;
          font-size: $text-font * 2rpx;
        }
      }
    }
  }
}

/* Facilities Section */
.facilities-section {
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

  .facilities-list {
    display: flex;
    flex-wrap: wrap;
    padding: 20rpx;

    .facility-item {
      display: flex;
      flex-direction: column;
      align-items: center;
      width: 20%;
      margin-bottom: 20rpx;

      .icon {
        font-size: $text-font * 2.9rpx;
        color: $mtRed-color;
        margin-bottom: 10rpx;
      }

      span {
        font-size: $text-font * 1.7rpx;
        text-align: center;
        color: $textBlack-color;
      }
    }
  }
}

/* Reviews Section */
.reviews-section {
  background-color: $nav-bgcolor;

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
</style>