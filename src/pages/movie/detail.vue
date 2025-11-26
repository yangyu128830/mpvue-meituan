<template>
  <div class="movie-detail">
    <!-- 电影海报 -->
    <div class="poster-banner">
      <img :src="movie.poster" alt="{{ movie.title }}" class="poster-img">
      <div class="poster-info">
        <div class="movie-title">{{ movie.title }}</div>
        <div class="movie-subtitle">{{ movie.subtitle }}</div>
        <div class="movie-price">{{ movie.price }}</div>
        <button class="buy-ticket-btn">立即购票</button>
      </div>
    </div>
    <!-- 电影详情 -->
    <div class="movie-info">
      <h2 class="section-title">电影详情</h2>
      <div class="info-row">
        <span class="info-label">类型:</span>
        <span class="info-value">{{ movie.genre }}</span>
      </div>
      <div class="info-row">
        <span class="info-label">导演:</span>
        <span class="info-value">{{ movie.director }}</span>
      </div>
      <div class="info-row">
        <span class="info-label">主演:</span>
        <span class="info-value">{{ movie.actors }}</span>
      </div>
      <div class="info-row">
        <span class="info-label">上映时间:</span>
        <span class="info-value">{{ movie.releaseDate }}</span>
      </div>
      <div class="info-row">
        <span class="info-label">评分:</span>
        <span class="info-value">{{ movie.rating }}</span>
      </div>
      <div class="info-row">
        <span class="info-label">简介:</span>
        <span class="info-value">{{ movie.description }}</span>
      </div>
    </div>
    <!-- 推荐影片 -->
    <div class="recommended-movies">
      <h2 class="section-title">推荐影片</h2>
      <div class="card-list">
        <div class="card" v-for="(movie, index) in recommendedMovies" :key="index">
          <img :src="movie.poster" alt="{{ movie.title }}" class="card-img">
          <div class="card-title">{{ movie.title }}</div>
          <div class="card-subtitle">{{ movie.subtitle }}</div>
          <div class="card-price">{{ movie.price }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      movie: {},
      recommendedMovies: [
        { poster: 'https://picsum.photos/id/1000/300/450', title: '星际穿越', subtitle: '科幻/冒险', rating: '9.4', price: '¥29.9', tag: '热映' },
        { poster: 'https://picsum.photos/id/1001/300/450', title: '霸王别姬', subtitle: '剧情/爱情', rating: '9.6', price: '¥39.9', tag: '经典' },
        { poster: 'https://picsum.photos/id/1002/300/450', title: '阿甘正传', subtitle: '剧情/励志', rating: '9.5', price: '¥49.9', tag: '经典' }
      ]
    }
  },
  onLoad(options) {
    if (options.movie) {
      this.movie = JSON.parse(options.movie);
    }
  },
  methods: {
    gotoDetail(movie) {
      wx.navigateTo({
        url: `/pages/movie/detail?movie=${JSON.stringify(movie)}`
      })
    }
  }
};
</script>

<style lang="scss" scoped>
@import '../../assets/global.scss';
@import '../../assets/iconfont.scss';

.movie-detail {
  padding-bottom: 60px;
}

.poster-banner {
  position: relative;
}

.poster-img {
  width: 100%;
  height: 400rpx;
  object-fit: cover;
}

.poster-info {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  background: linear-gradient(to top, rgba(0, 0, 0, 0.8), rgba(0, 0, 0, 0));
  padding: 80rpx 30rpx 30rpx;
  color: #fff;
}

.movie-title {
  font-size: 36rpx;
  font-weight: bold;
  margin-bottom: 10rpx;
}

.movie-subtitle {
  font-size: 24rpx;
  margin-bottom: 15rpx;
}

.movie-price {
  font-size: 30rpx;
  color: $mtRed-color;
  margin-bottom: 20rpx;
}

.buy-ticket-btn {
  background-color: $theme-color;
  color: #fff;
  border: none;
  padding: 15rpx 30rpx;
  border-radius: 8rpx;
  font-size: 28rpx;
  box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.1);
}

.movie-info {
  background-color: $nav-bgcolor;
  margin-bottom: 20rpx;
}

.section-title {
  font-size: 32rpx;
  font-weight: bold;
  padding: 20rpx 30rpx;
  border-bottom: 1rpx solid $spLine-color;
}

.info-row {
  display: flex;
  padding: 20rpx 30rpx;
  border-bottom: 1rpx solid $spLine-color;
}

.info-label {
  width: 120rpx;
  color: $textGray-color;
  font-size: 26rpx;
}

.info-value {
  flex: 1;
  font-size: 26rpx;
  color: $textBlack-color;
}

.recommended-movies {
  background-color: $nav-bgcolor;
}

.card-list {
  display: flex;
  flex-wrap: wrap;
  padding: 20rpx 30rpx;
}

.card {
  width: 32%;
  margin-right: 2%;
  margin-bottom: 20rpx;
  background-color: $nav-bgcolor;
  border-radius: 10rpx;
  overflow: hidden;
  transition: transform 0.3s ease;
  box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.05);
  &:nth-child(3n) {
    margin-right: 0;
  }
}

.card:hover {
  transform: translateY(-5rpx);
  box-shadow: 0 5rpx 20rpx rgba(0, 0, 0, 0.1);
}

.card-img {
  width: 100%;
  height: 180rpx;
  object-fit: cover;
  border-radius: 10rpx 10rpx 0 0;
}

.card-title {
  font-size: 24rpx;
  font-weight: bold;
  color: $textBlack-color;
  margin-bottom: 8rpx;
  padding: 10rpx 15rpx 0;
}

.card-subtitle {
  font-size: 22rpx;
  color: $textGray-color;
  margin-bottom: 8rpx;
  padding: 0 15rpx;
}

.card-price {
  font-size: 24rpx;
  color: $mtRed-color;
  font-weight: bold;
  padding: 0 15rpx 15rpx;
}
</style>