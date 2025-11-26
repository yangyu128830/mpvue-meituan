<template>
  <div class="movie-page">
    <!-- 顶部导航栏 -->
    <div class="header-c">
      <div class="header-l" @click="addressClick">
        <i class="icon mt-location-o" :style="{color: '#434343', 'font-size': 38 + 'rpx'}"></i>
        <span>上海市漕河泾开发区</span>
        <i class="icon mt-arrow-right-o" :style="{color: '#434343', 'font-size': 28 + 'rpx'}"></i>
      </div>
      <div class="header-r" @click="searchClick">
        <i class="icon mt-search-o"></i>
        <span>搜索电影、演出</span>
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
import '../../assets/iconfont.scss'

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
    addressClick() {
      // 地址点击事件处理
      console.log('Address clicked');
    },
    searchClick() {
      // 搜索点击事件处理
      console.log('Search clicked');
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
  background-color: $page-bgcolor;
  min-height: 100vh;
}

/* 顶部导航栏 */
.header-c {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 20rpx 30rpx;
  background-color: $nav-bgcolor;
  color: $nav-titlecolor;
}

.header-l {
  display: flex;
  align-items: center;
}

.header-l .icon {
  margin-right: 10rpx;
}

.header-l span {
  font-size: 28rpx;
  color: $textBlack-color;
}

.header-r {
  display: flex;
  align-items: center;
  background-color: #f5f5f5;
  border-radius: 20rpx;
  padding: 10rpx 20rpx;
  width: 60%;
}

.header-r .icon {
  margin-right: 10rpx;
  color: $textGray-color;
}

.header-r span {
  font-size: 24rpx;
  color: $textGray-color;
}

/* 分类标签 */
.category-tabs {
  display: flex;
  background-color: $nav-bgcolor;
  overflow-x: auto;
  white-space: nowrap;
  padding: 15rpx 0;
  border-bottom: 1rpx solid $spLine-color;
}

.category-tabs .tab {
  padding: 15rpx 30rpx;
  font-size: 28rpx;
  color: $textBlack-color;
  cursor: pointer;
  transition: all 0.3s ease;
  border-bottom: 4rpx solid transparent;
}

.category-tabs .tab.active {
  color: $theme-color;
  border-bottom: 4rpx solid $theme-color;
  font-weight: bold;
}

.category-tabs .tab:hover {
  color: $theme-color;
}

/* 内容区域 */
.content {
  padding: 0;
  background-color: $page-bgcolor;
  min-height: 60vh;
}

/* 区块样式 */
.section {
  background-color: $nav-bgcolor;
  margin-bottom: 20rpx;
  overflow: hidden;
}

.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20rpx 30rpx;
  border-bottom: 1rpx solid $spLine-color;
}

.section-title {
  font-size: 32rpx;
  font-weight: bold;
  color: $textBlack-color;
}

.section-more {
  color: $textGray-color;
  font-size: 24rpx;
  text-decoration: none;
}

/* 卡片列表 */
.card-list {
  display: flex;
  flex-wrap: wrap;
  padding: 20rpx 30rpx;
}

/* 卡片样式 */
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
  top: 10rpx;
  left: 10rpx;
  background-color: $theme-color;
  color: #fff;
  font-size: 20rpx;
  padding: 4rpx 10rpx;
  border-radius: 6rpx;
}

.card-info {
  padding: 15rpx;
}

.card-title {
  font-size: 24rpx;
  font-weight: bold;
  color: $textBlack-color;
  margin-bottom: 8rpx;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.card-subtitle {
  font-size: 22rpx;
  color: $textDarkGray-color;
  margin-bottom: 8rpx;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.card-rating {
  font-size: 22rpx;
  color: $theme-color;
  margin-bottom: 8rpx;
}

.rating-star {
  margin-left: 5rpx;
}

.card-release, .card-date {
  font-size: 22rpx;
  color: $textDarkGray-color;
  margin-bottom: 8rpx;
}

.card-price {
  font-size: 24rpx;
  color: $mtRed-color;
  font-weight: bold;
}

/* 响应式设计 */
@media (max-width: 750px) {
  .card {
    width: 48%;
    margin: 0 1% 20rpx;
  }
}

@media (max-width: 480px) {
  .card {
    width: 100%;
    margin: 0 0 20rpx;
  }
}

.bottom-ad {
  background-color: $nav-bgcolor;
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