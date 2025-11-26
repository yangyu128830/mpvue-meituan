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
.movie-detail {
  padding-bottom: 60px;
}

.poster-banner {
  position: relative;
}

.poster-img {
  width: 100%;
  height: 300px;
  object-fit: cover;
}

.poster-info {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  background: linear-gradient(to top, rgba(0, 0, 0, 0.8), rgba(0, 0, 0, 0));
  padding: 60px 15px 15px;
  color: #fff;
}

.movie-title {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 5px;
}

.movie-subtitle {
  font-size: 16px;
  margin-bottom: 10px;
}

.movie-price {
  font-size: 18px;
  color: #ff4444;
  margin-bottom: 15px;
}

.buy-ticket-btn {
  background-color: #ff4444;
  color: #fff;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  font-size: 16px;
}

.movie-info {
  background-color: #fff;
  margin-bottom: 10px;
}

.section-title {
  font-size: 16px;
  font-weight: bold;
  padding: 15px;
  border-bottom: 1px solid #eee;
}

.info-row {
  display: flex;
  padding: 10px 15px;
  border-bottom: 1px solid #eee;
}

.info-label {
  width: 80px;
  color: #999;
}

.info-value {
  flex: 1;
}

.recommended-movies {
  background-color: #fff;
}

.card-list {
  display: flex;
  flex-wrap: wrap;
  padding: 0 15px 15px;
}

.card {
  width: 32%;
  margin-right: 2%;
  margin-bottom: 15px;
  &:nth-child(3n) {
    margin-right: 0;
  }
}

.card-img {
  width: 100%;
  height: 120px;
  object-fit: cover;
  border-radius: 5px;
}

.card-title {
  font-size: 14px;
  margin-top: 5px;
}

.card-subtitle {
  font-size: 12px;
  color: #999;
  margin-top: 3px;
}

.card-price {
  font-size: 14px;
  color: #ff4444;
  margin-top: 3px;
}
</style>