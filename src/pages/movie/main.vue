<template>
  <div class="movie-page">
    <!-- 顶部导航栏 -->
    <div class="top-nav">
      <div class="nav-item" v-for="(item, index) in navList" :key="index" :class="{ active: currentTab === index }" @click="switchTab(index)">
        {{ item.name }}
      </div>
    </div>
    <!-- 内容区域 -->
    <div class="content">
      <!-- 电影分类内容 -->
      <div class="movie-content" v-show="currentTab === 0">
        <!-- 热门电影演出 -->
        <div class="section">
          <h2 class="section-title">热门电影演出</h2>
          <div class="card-list">
            <div class="card" v-for="(movie, index) in hotMovies" :key="index" @click="gotoDetail(movie)">
              <img :src="movie.poster" alt="{{ movie.title }}" class="card-img">
              <div class="card-title">{{ movie.title }}</div>
              <div class="card-subtitle">{{ movie.subtitle }}</div>
              <div class="card-price">{{ movie.price }}</div>
            </div>
          </div>
        </div>
        <!-- 待映电影 -->
        <div class="section">
          <h2 class="section-title">待映电影</h2>
          <div class="card-list">
            <div class="card" v-for="(movie, index) in upcomingMovies" :key="index">
              <img :src="movie.poster" alt="{{ movie.title }}" class="card-img">
              <div class="card-title">{{ movie.title }}</div>
              <div class="card-subtitle">{{ movie.subtitle }}</div>
              <div class="card-price">{{ movie.price }}</div>
            </div>
          </div>
        </div>
        <!-- 热门推荐 -->
        <div class="section">
          <h2 class="section-title">热门推荐</h2>
          <div class="card-list">
            <div class="card" v-for="(movie, index) in recommendMovies" :key="index">
              <img :src="movie.poster" alt="{{ movie.title }}" class="card-img">
              <div class="card-title">{{ movie.title }}</div>
              <div class="card-subtitle">{{ movie.subtitle }}</div>
              <div class="card-price">{{ movie.price }}</div>
            </div>
          </div>
        </div>
      </div>
      <!-- 演唱会分类内容 -->
      <div class="concert-content" v-show="currentTab === 1">
        <div class="section">
          <h2 class="section-title">热门演唱会</h2>
          <div class="card-list">
            <div class="card" v-for="(concert, index) in hotConcerts" :key="index" @click="gotoDetail(concert)">
              <img :src="concert.poster" alt="{{ concert.title }}" class="card-img">
              <div class="card-title">{{ concert.title }}</div>
              <div class="card-subtitle">{{ concert.subtitle }}</div>
              <div class="card-price">{{ concert.price }}</div>
            </div>
          </div>
        </div>
      </div>
      <!-- 演出分类内容 -->
      <div class="show-content" v-show="currentTab === 2">
        <div class="section">
          <h2 class="section-title">热门演出</h2>
          <div class="card-list">
            <div class="card" v-for="(show, index) in hotShows" :key="index" @click="gotoDetail(show)">
              <img :src="show.poster" alt="{{ show.title }}" class="card-img">
              <div class="card-title">{{ show.title }}</div>
              <div class="card-subtitle">{{ show.subtitle }}</div>
              <div class="card-price">{{ show.price }}</div>
            </div>
          </div>
        </div>
      </div>
      <!-- 脱口秀分类内容 -->
      <div class="talkshow-content" v-show="currentTab === 3">
        <div class="section">
          <h2 class="section-title">热门脱口秀</h2>
          <div class="card-list">
            <div class="card" v-for="(talkshow, index) in hotTalkshows" :key="index" @click="gotoDetail(talkshow)">
              <img :src="talkshow.poster" alt="{{ talkshow.title }}" class="card-img">
              <div class="card-title">{{ talkshow.title }}</div>
              <div class="card-subtitle">{{ talkshow.subtitle }}</div>
              <div class="card-price">{{ talkshow.price }}</div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- 底部广告和优惠团购 -->
    <div class="bottom-ad">
      <div class="ad-banner">
        <img src="/static/images/ad_movie.jpg" alt="新电影推荐">
      </div>
      <div class="discount-group">
        <h2 class="section-title">优惠团购</h2>
        <div class="card-list">
          <div class="card" v-for="(group, index) in discountGroups" :key="index">
            <img :src="group.poster" alt="{{ group.title }}" class="card-img">
            <div class="card-title">{{ group.title }}</div>
            <div class="card-subtitle">{{ group.subtitle }}</div>
            <div class="card-price">{{ group.price }}</div>
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
        { poster: '/static/images/icon_tabbar_home@3x.png', title: '电影1', subtitle: '类型1', price: '¥29.9' },
        { poster: '/static/images/icon_tabbar_order@3x.png', title: '电影2', subtitle: '类型2', price: '¥39.9' },
        { poster: '/static/images/icon_tabbar_mine@3x.png', title: '电影3', subtitle: '类型3', price: '¥49.9' }
      ],
      // 待映电影数据
      upcomingMovies: [
        { poster: '/static/images/icon_tabbar_home@3x.png', title: '电影4', subtitle: '类型4', price: '¥39.9' },
        { poster: '/static/images/icon_tabbar_order@3x.png', title: '电影5', subtitle: '类型5', price: '¥49.9' }
      ],
      // 热门推荐电影数据
      recommendMovies: [
        { poster: '/static/images/icon_tabbar_home@3x.png', title: '电影6', subtitle: '类型6', price: '¥29.9' },
        { poster: '/static/images/icon_tabbar_order@3x.png', title: '电影7', subtitle: '类型7', price: '¥39.9' },
        { poster: '/static/images/icon_tabbar_mine@3x.png', title: '电影8', subtitle: '类型8', price: '¥49.9' }
      ],
      // 热门演唱会数据
      hotConcerts: [
        { poster: '/static/images/icon_tabbar_home@3x.png', title: '演唱会1', subtitle: '歌手1', price: '¥199' },
        { poster: '/static/images/icon_tabbar_order@3x.png', title: '演唱会2', subtitle: '歌手2', price: '¥299' }
      ],
      // 热门演出数据
      hotShows: [
        { poster: '/static/images/icon_tabbar_home@3x.png', title: '演出1', subtitle: '类型1', price: '¥99' },
        { poster: '/static/images/icon_tabbar_order@3x.png', title: '演出2', subtitle: '类型2', price: '¥149' }
      ],
      // 热门脱口秀数据
      hotTalkshows: [
        { poster: '/static/images/icon_tabbar_home@3x.png', title: '脱口秀1', subtitle: '演员1', price: '¥129' },
        { poster: '/static/images/icon_tabbar_order@3x.png', title: '脱口秀2', subtitle: '演员2', price: '¥159' }
      ],
      // 优惠团购数据
      discountGroups: [
        { poster: '/static/images/icon_tabbar_home@3x.png', title: '团购1', subtitle: '优惠1', price: '¥59.9' },
        { poster: '/static/images/icon_tabbar_order@3x.png', title: '团购2', subtitle: '优惠2', price: '¥69.9' }
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
  padding-bottom: 60px;
}

.top-nav {
  display: flex;
  background-color: #fff;
  border-bottom: 1px solid #eee;
  padding: 0 15px;
}

.nav-item {
  flex: 1;
  text-align: center;
  padding: 15px 0;
  font-size: 14px;
  color: #999;
  position: relative;
  &.active {
    color: #000;
    &::after {
      content: '';
      position: absolute;
      bottom: 0;
      left: 50%;
      transform: translateX(-50%);
      width: 20px;
      height: 2px;
      background-color: #000;
    }
  }
}

.content {
  background-color: #f5f5f5;
  min-height: 60vh;
}

.section {
  background-color: #fff;
  margin-bottom: 10px;
}

.section-title {
  font-size: 16px;
  font-weight: bold;
  padding: 15px;
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

.bottom-ad {
  background-color: #fff;
  margin-top: 10px;
}

.ad-banner {
  padding: 15px;
  img {
    width: 100%;
    border-radius: 5px;
  }
}
</style>