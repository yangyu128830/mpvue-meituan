<template>
  <div class="movie-page">
    <!-- 顶部导航栏 -->
    <div class="nav-bar">
      <div class="nav-title">电影演出</div>
      <div class="nav-search">
        <input type="text" placeholder="搜索电影、演出">
      </div>
    </div>
    <!-- 分类标签 -->
    <div class="category-tabs">
      <div class="tab" v-for="(item, index) in navList" :key="index" :class="{ active: currentTab === index }" @click="switchTab(index)">
        {{ item.name }}
      </div>
    </div>
    <!-- 内容区域 -->
    <div class="content">
      <!-- 电影分类内容 -->
      <div class="movie-content" v-show="currentTab === 0">
        <!-- 热门电影演出 -->
        <div class="section">
          <div class="section-header">
            <h2 class="section-title">热门电影演出</h2>
            <a href="javascript:void(0)" class="section-more">更多</a>
          </div>
          <div class="card-list">
            <div class="card" v-for="(movie, index) in hotMovies" :key="index" @click="gotoDetail(movie)">
              <div class="card-poster">
                <img :src="movie.poster" alt="{{ movie.title }}" class="card-img">
                <div class="card-tag" v-if="movie.tag">{{ movie.tag }}</div>
              </div>
              <div class="card-info">
                <div class="card-title">{{ movie.title }}</div>
                <div class="card-subtitle">{{ movie.subtitle }}</div>
                <div class="card-rating" v-if="movie.rating">
                  <span>{{ movie.rating }}</span>
                  <span class="rating-star">★</span>
                </div>
                <div class="card-price">{{ movie.price }}</div>
              </div>
            </div>
          </div>
        </div>
        <!-- 待映电影 -->
        <div class="section">
          <div class="section-header">
            <h2 class="section-title">待映电影</h2>
            <a href="javascript:void(0)" class="section-more">更多</a>
          </div>
          <div class="card-list">
            <div class="card" v-for="(movie, index) in upcomingMovies" :key="index" @click="gotoDetail(movie)">
              <div class="card-poster">
                <img :src="movie.poster" alt="{{ movie.title }}" class="card-img">
                <div class="card-tag" v-if="movie.tag">{{ movie.tag }}</div>
              </div>
              <div class="card-info">
                <div class="card-title">{{ movie.title }}</div>
                <div class="card-subtitle">{{ movie.subtitle }}</div>
                <div class="card-release" v-if="movie.releaseDate">
                  {{ movie.releaseDate }}
                </div>
                <div class="card-price">{{ movie.price }}</div>
              </div>
            </div>
          </div>
        </div>
        <!-- 热门推荐 -->
        <div class="section">
          <div class="section-header">
            <h2 class="section-title">热门推荐</h2>
            <a href="javascript:void(0)" class="section-more">更多</a>
          </div>
          <div class="card-list">
            <div class="card" v-for="(movie, index) in recommendMovies" :key="index" @click="gotoDetail(movie)">
              <div class="card-poster">
                <img :src="movie.poster" alt="{{ movie.title }}" class="card-img">
                <div class="card-tag" v-if="movie.tag">{{ movie.tag }}</div>
              </div>
              <div class="card-info">
                <div class="card-title">{{ movie.title }}</div>
                <div class="card-subtitle">{{ movie.subtitle }}</div>
                <div class="card-rating" v-if="movie.rating">
                  <span>{{ movie.rating }}</span>
                  <span class="rating-star">★</span>
                </div>
                <div class="card-price">{{ movie.price }}</div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- 演唱会分类内容 -->
      <div class="concert-content" v-show="currentTab === 1">
        <div class="section">
          <div class="section-header">
            <h2 class="section-title">热门演唱会</h2>
            <a href="javascript:void(0)" class="section-more">更多</a>
          </div>
          <div class="card-list">
            <div class="card" v-for="(concert, index) in hotConcerts" :key="index" @click="gotoDetail(concert)">
              <div class="card-poster">
                <img :src="concert.poster" alt="{{ concert.title }}" class="card-img">
                <div class="card-tag" v-if="concert.tag">{{ concert.tag }}</div>
              </div>
              <div class="card-info">
                <div class="card-title">{{ concert.title }}</div>
                <div class="card-subtitle">{{ concert.subtitle }}</div>
                <div class="card-date" v-if="concert.date">
                  {{ concert.date }}
                </div>
                <div class="card-price">{{ concert.price }}</div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- 演出分类内容 -->
      <div class="show-content" v-show="currentTab === 2">
        <div class="section">
          <div class="section-header">
            <h2 class="section-title">热门演出</h2>
            <a href="javascript:void(0)" class="section-more">更多</a>
          </div>
          <div class="card-list">
            <div class="card" v-for="(show, index) in hotShows" :key="index" @click="gotoDetail(show)">
              <div class="card-poster">
                <img :src="show.poster" alt="{{ show.title }}" class="card-img">
                <div class="card-tag" v-if="show.tag">{{ show.tag }}</div>
              </div>
              <div class="card-info">
                <div class="card-title">{{ show.title }}</div>
                <div class="card-subtitle">{{ show.subtitle }}</div>
                <div class="card-date" v-if="show.date">
                  {{ show.date }}
                </div>
                <div class="card-price">{{ show.price }}</div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- 脱口秀分类内容 -->
      <div class="talkshow-content" v-show="currentTab === 3">
        <div class="section">
          <div class="section-header">
            <h2 class="section-title">热门脱口秀</h2>
            <a href="javascript:void(0)" class="section-more">更多</a>
          </div>
          <div class="card-list">
            <div class="card" v-for="(talkshow, index) in hotTalkshows" :key="index" @click="gotoDetail(talkshow)">
              <div class="card-poster">
                <img :src="talkshow.poster" alt="{{ talkshow.title }}" class="card-img">
                <div class="card-tag" v-if="talkshow.tag">{{ talkshow.tag }}</div>
              </div>
              <div class="card-info">
                <div class="card-title">{{ talkshow.title }}</div>
                <div class="card-subtitle">{{ talkshow.subtitle }}</div>
                <div class="card-date" v-if="talkshow.date">
                  {{ talkshow.date }}
                </div>
                <div class="card-price">{{ talkshow.price }}</div>
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
      currentTab: 0,
      navList: [
        { name: '电影' },
        { name: '演唱会' },
        { name: '演出' },
        { name: '脱口秀' }
      ],
      // 热门电影演出数据
      hotMovies: [
        { poster: 'https://picsum.photos/id/1000/300/450', title: '星际穿越', subtitle: '科幻/冒险', rating: '9.4', price: '¥29.9', tag: '热映' },
        { poster: 'https://picsum.photos/id/1001/300/450', title: '霸王别姬', subtitle: '剧情/爱情', rating: '9.6', price: '¥39.9', tag: '经典' },
        { poster: 'https://picsum.photos/id/1002/300/450', title: '阿甘正传', subtitle: '剧情/励志', rating: '9.5', price: '¥49.9', tag: '经典' },
        { poster: 'https://picsum.photos/id/1003/300/450', title: '盗梦空间', subtitle: '科幻/悬疑', rating: '9.4', price: '¥34.9', tag: '热映' }
      ],
      // 待映电影数据
      upcomingMovies: [
        { poster: 'https://picsum.photos/id/1004/300/450', title: '新蝙蝠侠', subtitle: '动作/科幻', releaseDate: '2023-03-04', price: '¥39.9', tag: '预售' },
        { poster: 'https://picsum.photos/id/1005/300/450', title: '蜘蛛侠：英雄无归', subtitle: '动作/冒险', releaseDate: '2023-01-14', price: '¥49.9', tag: '预售' },
        { poster: 'https://picsum.photos/id/1006/300/450', title: '奇异博士2', subtitle: '奇幻/冒险', releaseDate: '2023-05-06', price: '¥44.9', tag: '预售' }
      ],
      // 热门推荐电影数据
      recommendMovies: [
        { poster: 'https://picsum.photos/id/1007/300/450', title: '教父', subtitle: '剧情/犯罪', rating: '9.5', price: '¥29.9', tag: '经典' },
        { poster: 'https://picsum.photos/id/1008/300/450', title: '肖申克的救赎', subtitle: '剧情/犯罪', rating: '9.7', price: '¥39.9', tag: '经典' },
        { poster: 'https://picsum.photos/id/1009/300/450', title: '泰坦尼克号', subtitle: '爱情/灾难', rating: '9.4', price: '¥49.9', tag: '经典' },
        { poster: 'https://picsum.photos/id/1010/300/450', title: '辛德勒的名单', subtitle: '历史/战争', rating: '9.5', price: '¥34.9', tag: '经典' }
      ],
      // 热门演唱会数据
      hotConcerts: [
        { poster: 'https://picsum.photos/id/1011/300/450', title: '周杰伦2023演唱会', subtitle: '北京/上海/广州', date: '2023-05-10至2023-05-20', price: '¥199-1299', tag: '热卖' },
        { poster: 'https://picsum.photos/id/1012/300/450', title: '陈奕迅FEAR and DREAMS演唱会', subtitle: '香港/澳门', date: '2023-06-01至2023-06-15', price: '¥299-1599', tag: '预售' }
      ],
      // 热门演出数据
      hotShows: [
        { poster: 'https://picsum.photos/id/1013/300/450', title: '开心麻花《乌龙山伯爵》', subtitle: '喜剧/舞台剧', date: '2023-04-15至2023-04-20', price: '¥99-599', tag: '热卖' },
        { poster: 'https://picsum.photos/id/1014/300/450', title: '国家大剧院《天鹅湖》', subtitle: '芭蕾舞/经典', date: '2023-05-01至2023-05-05', price: '¥149-899', tag: '热卖' }
      ],
      // 热门脱口秀数据
      hotTalkshows: [
        { poster: 'https://picsum.photos/id/1015/300/450', title: '李诞脱口秀专场', subtitle: '上海/北京', date: '2023-04-20至2023-04-25', price: '¥129-699', tag: '热卖' },
        { poster: 'https://picsum.photos/id/1016/300/450', title: '池子脱口秀巡演', subtitle: '成都/重庆', date: '2023-05-15至2023-05-20', price: '¥159-799', tag: '预售' }
      ]
    }
  },
  methods: {
    switchTab(index) {
      this.currentTab = index;
    },
    gotoDetail(movie) {
      wx.navigateTo({
        url: `/pages/movie/detail?movie=${JSON.stringify(movie)}`
      })
    }
  }
};
</script>

<style lang="scss" scoped>
.movie-page {
  background-color: #f5f5f5;
  min-height: 100vh;
}

/* 顶部导航栏 */
.nav-bar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 15px 20px;
  background-color: #ff4444;
  color: #fff;
}

.nav-title {
  font-size: 20px;
  font-weight: bold;
}

.nav-search {
  width: 200px;
  height: 30px;
  background-color: rgba(255, 255, 255, 0.3);
  border-radius: 15px;
  padding: 0 15px;
  display: flex;
  align-items: center;
}

.nav-search input {
  width: 100%;
  background-color: transparent;
  border: none;
  outline: none;
  color: #fff;
  font-size: 14px;
}

.nav-search input::placeholder {
  color: rgba(255, 255, 255, 0.7);
}

/* 分类标签 */
.category-tabs {
  display: flex;
  background-color: #fff;
  overflow-x: auto;
  white-space: nowrap;
  padding: 10px 0;
  border-bottom: 1px solid #eee;
}

.category-tabs .tab {
  padding: 10px 20px;
  font-size: 16px;
  color: #333;
  cursor: pointer;
  transition: all 0.3s ease;
  border-bottom: 2px solid transparent;
}

.category-tabs .tab.active {
  color: #ff4444;
  border-bottom: 2px solid #ff4444;
  font-weight: bold;
}

.category-tabs .tab:hover {
  color: #ff4444;
}

/* 内容区域 */
.content {
  padding: 10px;
}

/* 区块样式 */
.section {
  background-color: #fff;
  border-radius: 8px;
  margin-bottom: 10px;
  overflow: hidden;
}

.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px 20px;
  border-bottom: 1px solid #f5f5f5;
}

.section-title {
  font-size: 18px;
  font-weight: bold;
  color: #333;
}

.section-more {
  color: #999;
  font-size: 14px;
  text-decoration: none;
}

/* 卡片列表 */
.card-list {
  display: flex;
  flex-wrap: wrap;
  padding: 10px;
}

/* 卡片样式 */
.card {
  width: 23%;
  margin: 0 1% 15px;
  background-color: #fff;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease;
}

.card:hover {
  transform: translateY(-5px);
}

.card-poster {
  position: relative;
  width: 100%;
  padding-bottom: 140%;
  overflow: hidden;
}

.card-img {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.card-tag {
  position: absolute;
  top: 10px;
  left: 10px;
  background-color: #ff4444;
  color: #fff;
  font-size: 12px;
  padding: 2px 8px;
  border-radius: 4px;
}

.card-info {
  padding: 10px;
}

.card-title {
  font-size: 14px;
  font-weight: bold;
  color: #333;
  margin-bottom: 5px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.card-subtitle {
  font-size: 12px;
  color: #999;
  margin-bottom: 5px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.card-rating {
  font-size: 12px;
  color: #ff9900;
  margin-bottom: 5px;
}

.rating-star {
  margin-left: 3px;
}

.card-release, .card-date {
  font-size: 12px;
  color: #666;
  margin-bottom: 5px;
}

.card-price {
  font-size: 14px;
  color: #ff4444;
  font-weight: bold;
}

/* 响应式设计 */
@media (max-width: 750px) {
  .card {
    width: 48%;
    margin: 0 1% 15px;
  }
}

@media (max-width: 480px) {
  .card {
    width: 100%;
    margin: 0 0 15px;
  }
}

.tabbar-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-size: 24rpx;
}

.tabbar-item.active {
  color: $theme-color;
}

.content {
  background-color: $pageBg-color;
  min-height: 60vh;
  padding-top: 20rpx;
}

.section {
  background-color: $bgWhite-color;
  margin-bottom: 20rpx;
  border-radius: 10rpx;
}

.section-title {
  font-size: 32rpx;
  font-weight: bold;
  padding: 30rpx;
}

.card-list {
  display: flex;
  flex-wrap: wrap;
  padding: 0 30rpx 30rpx;
}

.card {
  width: 32%;
  margin-right: 2%;
  margin-bottom: 30rpx;
  &:nth-child(3n) {
    margin-right: 0;
  }
}

.card-img {
  width: 100%;
  height: 240rpx;
  object-fit: cover;
  border-radius: 10rpx;
}

.card-title {
  font-size: 28rpx;
  margin-top: 10rpx;
}

.card-subtitle {
  font-size: 24rpx;
  color: $textGray-color;
  margin-top: 6rpx;
}

.card-price {
  font-size: 28rpx;
  color: $textRed-color;
  margin-top: 6rpx;
}

.bottom-ad {
  background-color: $bgWhite-color;
  margin-top: 20rpx;
  border-radius: 10rpx;
}

.ad-banner {
  padding: 30rpx;
  img {
    width: 100%;
    border-radius: 10rpx;
  }
}
</style>