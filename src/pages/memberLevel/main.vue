<template>
  <div class="container">
    <div class="content">
      <!-- Header -->
      <div class="header-c">
        <div class="header-l">
          <i class="icon mt-back" @click="goBack"></i>
        </div>
        <div class="header-m">
          <span class="header-title">会员中心</span>
        </div>
        <div class="header-r">
          <span class="header-action" @click="goToPointsCenter">积分中心</span>
        </div>
      </div>
      
      <!-- Level Card -->
      <div class="level-card-wrapper">
        <div class="level-card" :class="levelClass">
          <div class="card-top">
            <div class="current-level">
              <span class="level-label">当前等级</span>
              <h2 class="level-name">{{ levelName }}</h2>
              <div class="points-display">
                <span class="points-value">{{ points }}</span>
                <span class="points-label">积分</span>
              </div>
            </div>
            <div class="level-icon">
              <img :src="getLevelIcon()" alt="Level Icon">
            </div>
          </div>
          
          <div class="progress-section">
            <div class="progress-bar-container">
              <div class="progress-text">
                <span>已获得 {{ points }} 积分</span>
                <span>共需 {{ nextLevelPoints }} 积分</span>
              </div>
              <div class="progress-bar">
                <div class="progress-fill" :style="{ width: progress + '%' }"></div>
              </div>
              <div class="next-level-info">
                距离 <span class="next-level-name">{{ nextLevelName }}</span> 还差 {{ nextLevelPoints - points }} 积分
              </div>
            </div>
          </div>
        </div>
      </div>
      
      <!-- 阶段权益 -->
      <div class="stage-benefits">
        <div class="section">
          <div class="l"></div>
          <div class="m">各等级权益</div>
          <div class="r"></div>
        </div>
        <div class="stage-tabs">
          <div class="stage-tab" :class="{ active: activeStage === 0 }" @click="activeStage = 0">白银会员</div>
          <div class="stage-tab" :class="{ active: activeStage === 1 }" @click="activeStage = 1">黄金会员</div>
          <div class="stage-tab" :class="{ active: activeStage === 2 }" @click="activeStage = 2">黑金会员</div>
        </div>
        <div class="stage-content">
          <div v-if="activeStage === 0" class="stage-info">
            <div class="stage-benefit-item">每月50元代金券</div>
            <div class="stage-benefit-item">会员价商品95折</div>
            <div class="stage-benefit-item">免费配送服务</div>
          </div>
          <div v-if="activeStage === 1" class="stage-info">
            <div class="stage-benefit-item">每月100元代金券</div>
            <div class="stage-benefit-item">会员价商品9折</div>
            <div class="stage-benefit-item">免费配送服务</div>
            <div class="stage-benefit-item">优先预订服务</div>
          </div>
          <div v-if="activeStage === 2" class="stage-info">
            <div class="stage-benefit-item">每年免费酒店入住一次</div>
            <div class="stage-benefit-item">每月200元代金券</div>
            <div class="stage-benefit-item">会员价商品8折</div>
            <div class="stage-benefit-item">专属客服服务</div>
            <div class="stage-benefit-item">优先预订服务</div>
          </div>
        </div>
      </div>
      
      <!-- 福利商品板块 -->
      <div class="welfare-products">
        <div class="section">
          <div class="l"></div>
          <div class="m">福利商品</div>
          <div class="r"></div>
        </div>
        <div class="product-list">
          <div class="product-item" v-for="product in welfareProducts" :key="product.id">
            <img :src="product.image" class="product-image" />
            <div class="product-info">
              <div class="product-name">{{ product.name }}</div>
              <div class="product-price">
                <span class="current-price">{{ product.currentPrice }}</span>
                <span class="original-price">{{ product.originalPrice }}</span>
              </div>
              <div class="product-desc">{{ product.desc }}</div>
            </div>
          </div>
        </div>
      </div>
      
      <!-- 商品优惠券 -->
      <div class="product-coupons">
        <div class="section">
          <div class="l"></div>
          <div class="m">商品优惠券</div>
          <div class="r"></div>
        </div>
        <div class="coupon-list">
          <div class="coupon-item" v-for="coupon in productCoupons" :key="coupon.id">
            <div class="coupon-amount">{{ coupon.amount }}</div>
            <div class="coupon-desc">{{ coupon.desc }}</div>
            <div class="coupon-validity">有效期至: {{ coupon.validity }}</div>
            <button class="coupon-btn" :disabled="coupon.used" @click="claimCoupon(coupon)">
              {{ coupon.used ? '已使用' : '立即领取' }}
            </button>
          </div>
        </div>
      </div>
      
      <!-- 会员等级对应的商品列表 -->
      <div class="member-products">
        <div class="section">
          <div class="l"></div>
          <div class="m">会员专享商品</div>
          <div class="r"></div>
        </div>
        <div class="product-categories">
          <div class="category-item" :class="{ active: activeCategory === 0 }" @click="activeCategory = 0">附近</div>
          <div class="category-item" :class="{ active: activeCategory === 1 }" @click="activeCategory = 1">全部品类</div>
          <div class="category-item" :class="{ active: activeCategory === 2 }" @click="activeCategory = 2">智能排序</div>
          <div class="category-item" :class="{ active: activeCategory === 3 }" @click="activeCategory = 3">必吃榜</div>
        </div>
        <div class="product-list">
          <div class="product-item" v-for="product in memberProducts" :key="product.id">
            <img :src="product.image" class="product-image" />
            <div class="product-info">
              <div class="product-name">{{ product.name }}</div>
              <div class="product-price">
                <span class="current-price">{{ product.currentPrice }}</span>
                <span class="original-price">{{ product.originalPrice }}</span>
              </div>
              <div class="product-desc">{{ product.desc }}</div>
              <div class="product-discount">会员专享</div>
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
      points: 800, // 模拟当前积分
      levelName: '',
      levelClass: '',
      nextLevelName: '',
      nextLevelPoints: 0,
      progress: 0,
      benefits: [],
      // 新添加的属性
      activeStage: 0, // 0:白银, 1:黄金, 2:黑金
      activeCategory: 0, // 0:附近, 1:全部品类, 2:智能排序, 3:必吃榜
      // 福利商品数据
      welfareProducts: [
        {
          id: 1,
          image: 'https://via.placeholder.com/100',
          name: '精选水果拼盘',
          currentPrice: '¥29.9',
          originalPrice: '¥59.9',
          desc: '新鲜水果组合，会员专享'
        },
        {
          id: 2,
          image: 'https://via.placeholder.com/100',
          name: '豪华双人餐',
          currentPrice: '¥128',
          originalPrice: '¥198',
          desc: '双人尊享套餐，会员专享'
        },
        {
          id: 3,
          image: 'https://via.placeholder.com/100',
          name: '下午茶套餐',
          currentPrice: '¥49',
          originalPrice: '¥89',
          desc: '休闲下午茶，会员专享'
        }
      ],
      // 商品优惠券数据
      productCoupons: [
        {
          id: 1,
          amount: '满100减50',
          desc: '全场通用',
          validity: '2023-12-31',
          used: false
        },
        {
          id: 2,
          amount: '满200减100',
          desc: '美食专区',
          validity: '2023-12-31',
          used: true
        },
        {
          id: 3,
          amount: '满500减200',
          desc: '酒店专区',
          validity: '2023-12-31',
          used: false
        }
      ],
      // 会员专享商品数据
      memberProducts: [
        {
          id: 1,
          image: 'https://via.placeholder.com/100',
          name: '招牌红烧肉',
          currentPrice: '¥38',
          originalPrice: '¥58',
          desc: '传统名菜，肥而不腻'
        },
        {
          id: 2,
          image: 'https://via.placeholder.com/100',
          name: '清蒸鲈鱼',
          currentPrice: '¥48',
          originalPrice: '¥78',
          desc: '新鲜鲈鱼，肉质鲜美'
        },
        {
          id: 3,
          image: 'https://via.placeholder.com/100',
          name: '佛跳墙',
          currentPrice: '¥98',
          originalPrice: '¥168',
          desc: '经典闽菜，滋补佳品'
        },
        {
          id: 4,
          image: 'https://via.placeholder.com/100',
          name: '麻辣火锅',
          currentPrice: '¥88',
          originalPrice: '¥138',
          desc: '正宗川味，麻辣鲜香'
        }
      ]
    }
  },
  onLoad() {
    this.calculateLevel()
  },
  methods: {
    calculateLevel() {
      if (this.points >= 10000) {
        this.levelName = '黑金会员'
        this.levelClass = 'black-gold'
        this.benefits = [
          { id: 1, icon: '🏨', title: '免费酒店', desc: '每年可免费入住任意酒店一次' },
          { id: 2, icon: '💳', title: '每月代金券', desc: '每月可领取200元代金券' },
          { id: 3, icon: '🎁', title: '专属礼品', desc: '生日专享豪华礼品' }
        ]
      } else if (this.points >= 1000) {
        this.levelName = '黄金会员'
        this.levelClass = 'gold'
        this.benefits = [
          { id: 1, icon: '🎟️', title: '每月代金券', desc: '每月可领取100元代金券' },
          { id: 2, icon: '🚗', title: '免费配送', desc: '每月5次免费配送服务' },
          { id: 3, icon: '⏱️', title: '优先预订', desc: '热门商品优先预订权' }
        ]
      } else if (this.points >= 500) {
        this.levelName = '白银会员'
        this.levelClass = 'silver'
        this.benefits = [
          { id: 1, icon: '🎟️', title: '每月代金券', desc: '每月可领取50元代金券' },
          { id: 2, icon: '🚗', title: '免费配送', desc: '每月2次免费配送服务' }
        ]
      } else {
        this.levelName = '普通会员'
        this.levelClass = 'normal'
        this.benefits = []
      }

      // Calculate progress to next level
      if (this.points < 500) {
        this.nextLevelName = '白银会员'
        this.nextLevelPoints = 500
      } else if (this.points < 1000) {
        this.nextLevelName = '黄金会员'
        this.nextLevelPoints = 1000
      } else if (this.points < 10000) {
        this.nextLevelName = '黑金会员'
        this.nextLevelPoints = 10000
      } else {
        this.nextLevelName = '顶级会员'
        this.nextLevelPoints = 10000
      }

      if (this.points < this.nextLevelPoints) {
        this.progress = (this.points / this.nextLevelPoints) * 100
      } else {
        this.progress = 100
      }
    },
    getLevelIcon() {
      // Return appropriate icon based on level
      switch (this.levelName) {
        case '黑金会员':
          return 'https://via.placeholder.com/80/000000/ffffff?text=HG'
        case '黄金会员':
          return 'https://via.placeholder.com/80/FFD700/000000?text=G'
        case '白银会员':
          return 'https://via.placeholder.com/80/C0C0C0/000000?text=S'
        default:
          return 'https://via.placeholder.com/80/EEEEEE/000000?text=N'
      }
    },
    goBack() {
      wx.navigateBack()
    },
    goToPointsCenter() {
      wx.navigateTo({ url: '/pages/pointsCenter/main' })
    },
    claimCoupon(coupon) {
      wx.showToast({
        title: '领取成功',
        icon: 'success',
        duration: 2000
      })
      coupon.used = true
    }
  }
}
</script>

<style lang="scss" scoped>
.container {
  .content {
    display: flex;
    flex-direction: column;
    position: relative;
    background-color: #f5f5f5;
    min-height: 100vh;
    
    /* Header */
    .header-c {
      display: flex;
      align-items: center;
      height: 80rpx;
      position: fixed;
      width: auto;
      padding: 0 30rpx;
      width: 100%;
      box-sizing: border-box;
      background-color: white;
      z-index: 999;
      
      .header-l {
        display: flex;
        align-items: center;
      }
      
      .header-m {
        flex: 1;
        text-align: center;
        
        .header-title {
          font-size: 28rpx;
          color: #434343;
        }
      }
      
      .header-r {
        display: flex;
        align-items: center;
        
        .header-action {
          font-size: 24rpx;
          color: #FF6B00;
        }
      }
    }
    
    /* Level Card */
    .level-card-wrapper {
      padding: 16rpx;
      padding-top: 100rpx;
    }
    
    .level-card {
      border-radius: 12rpx;
      color: white;
      overflow: hidden;
      box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.15);

      &.normal {
        background: linear-gradient(135deg, #EEEEEE 0%, #DDDDDD 100%);
        color: #333;
      }

      &.silver {
        background: linear-gradient(135deg, #C0C0C0 0%, #A9A9A9 100%);
      }

      &.gold {
        background: linear-gradient(135deg, #FFD700 0%, #FFA500 100%);
        color: #333;
      }

      &.black-gold {
        background: linear-gradient(135deg, #000000 0%, #333333 100%);
      }

      .card-top {
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 24rpx 20rpx;
      }

      .current-level {
        .level-label {
          font-size: 14rpx;
          opacity: 0.8;
          margin-bottom: 8rpx;
          display: block;
        }

        .level-name {
          font-size: 28rpx;
          font-weight: bold;
          margin-bottom: 12rpx;
        }

        .points-display {
          display: flex;
          align-items: baseline;

          .points-value {
            font-size: 32rpx;
            font-weight: bold;
            margin-right: 8rpx;
          }

          .points-label {
            font-size: 14rpx;
            opacity: 0.8;
          }
        }
      }

      .level-icon {
        img {
          width: 80rpx;
          height: 80rpx;
          border-radius: 50%;
          box-shadow: 0 4rpx 8rpx rgba(0, 0, 0, 0.2);
        }
      }

      .progress-section {
        padding: 20rpx;
        background-color: rgba(255, 255, 255, 0.1);

        .progress-bar-container {
          .progress-text {
            display: flex;
            justify-content: space-between;
            font-size: 14rpx;
            margin-bottom: 8rpx;
            opacity: 0.8;
          }

          .progress-bar {
            height: 12rpx;
            background-color: rgba(255, 255, 255, 0.2);
            border-radius: 6rpx;
            overflow: hidden;
            margin-bottom: 12rpx;

            .progress-fill {
              height: 100%;
              background-color: #fff;
              border-radius: 6rpx;
              transition: width 0.3s ease;
            }
          }

          .next-level-info {
            font-size: 14rpx;
            text-align: center;

            .next-level-name {
              font-weight: bold;
              margin: 0 4rpx;
            }
          }
        }
      }
    }
    
    /* 阶段权益 */
    .stage-benefits {
      margin: 20rpx;
      background-color: #fff;
      border-radius: 12rpx;
      overflow: hidden;
      box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.08);
      
      .section {
        display: flex;
        align-items: center;
        justify-content: center;
        padding: 20rpx 0;
        
        .l {
          height: 2rpx;
          width: 60rpx;
          background-color: #999;
        }
        
        .m {
          font-size: 32rpx;
          color: #333;
          margin: 0 20rpx;
          font-weight: bold;
        }
        
        .r {
          height: 2rpx;
          width: 60rpx;
          background-color: #999;
        }
      }
      
      .stage-tabs {
        display: flex;
        border-bottom: 1rpx solid #eee;
        
        .stage-tab {
          flex: 1;
          text-align: center;
          padding: 14rpx;
          cursor: pointer;
          font-size: 14rpx;
          color: #666;
          border-bottom: 2rpx solid transparent;
          position: relative;

          &.active {
            color: #FF6B00;
            border-bottom-color: #FF6B00;
          }
        }
      }
      
      .stage-content {
        padding: 24rpx 20rpx;
        
        .stage-benefit-item {
          font-size: 15rpx;
          margin-bottom: 16rpx;
          padding-left: 24rpx;
          position: relative;
          display: flex;
          align-items: center;

          &::before {
            content: '✓';
            position: absolute;
            left: 0;
            color: #4CAF50;
            font-size: 18rpx;
            margin-right: 8rpx;
          }
        }
      }
    }
    
    /* 福利商品板块 */
    .welfare-products,
    .member-products {
      margin: 20rpx;
      background-color: #fff;
      border-radius: 12rpx;
      overflow: hidden;
      box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.08);
      
      .section {
        display: flex;
        align-items: center;
        justify-content: center;
        padding: 20rpx 0;
        
        .l {
          height: 2rpx;
          width: 60rpx;
          background-color: #999;
        }
        
        .m {
          font-size: 32rpx;
          color: #333;
          margin: 0 20rpx;
          font-weight: bold;
        }
        
        .r {
          height: 2rpx;
          width: 60rpx;
          background-color: #999;
        }
      }
      
      .product-list {
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
        padding: 16rpx;
        
        .product-item {
          width: 48%;
          margin-bottom: 20rpx;
          display: flex;
          flex-direction: column;
          border-radius: 8rpx;
          overflow: hidden;
          background-color: #fafafa;
          transition: transform 0.2s ease;

          &:active {
            transform: scale(0.98);
          }

          .product-image {
            width: 100%;
            height: 200rpx;
            object-fit: cover;
          }

          .product-info {
            padding: 12rpx;
            flex: 1;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
          }

          .product-name {
            font-size: 24rpx;
            font-weight: 500;
            color: #333;
            margin-bottom: 6rpx;
            line-height: 1.3;
            display: -webkit-box;
            -webkit-line-clamp: 2;
            -webkit-box-orient: vertical;
            overflow: hidden;
          }

          .product-price {
            display: flex;
            align-items: baseline;
            margin-bottom: 4rpx;

            .current-price {
              font-size: 28rpx;
              font-weight: bold;
              color: #FF6B00;
              margin-right: 6rpx;
            }

            .original-price {
              font-size: 20rpx;
              color: #999;
              text-decoration: line-through;
            }
          }

          .product-desc {
            font-size: 20rpx;
            color: #666;
            margin-bottom: 4rpx;
          }

          .product-discount {
            font-size: 20rpx;
            color: #FF6B00;
            background-color: #FFF3E0;
            padding: 2rpx 6rpx;
            border-radius: 4rpx;
            align-self: flex-start;
          }
        }
      }
    }
    
    /* 商品优惠券 */
    .product-coupons {
      margin: 20rpx;
      background-color: #fff;
      border-radius: 12rpx;
      overflow: hidden;
      box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.08);
      
      .section {
        display: flex;
        align-items: center;
        justify-content: center;
        padding: 20rpx 0;
        
        .l {
          height: 2rpx;
          width: 60rpx;
          background-color: #999;
        }
        
        .m {
          font-size: 32rpx;
          color: #333;
          margin: 0 20rpx;
          font-weight: bold;
        }
        
        .r {
          height: 2rpx;
          width: 60rpx;
          background-color: #999;
        }
      }
      
      .coupon-list {
        display: flex;
        flex-direction: column;
        padding: 16rpx;
        
        .coupon-item {
          display: flex;
          align-items: center;
          margin-bottom: 20rpx;
          padding: 16rpx;
          border-radius: 8rpx;
          background: linear-gradient(135deg, #FFF5F5 0%, #FFECEC 100%);
          position: relative;
          overflow: hidden;

          &::before {
            content: '';
            position: absolute;
            top: 0;
            right: 0;
            width: 40rpx;
            height: 40rpx;
            background: linear-gradient(135deg, #FF4444 0%, #FF6B00 100%);
            border-radius: 0 0 0 8rpx;
          }

          .coupon-amount {
            font-size: 28rpx;
            font-weight: bold;
            color: #FF4444;
            margin-right: 20rpx;
            z-index: 1;
          }

          .coupon-desc {
            font-size: 24rpx;
            color: #333;
            flex: 1;
            z-index: 1;
          }

          .coupon-validity {
            font-size: 20rpx;
            color: #999;
            margin-right: 20rpx;
            z-index: 1;
          }

          .coupon-btn {
            padding: 8rpx 16rpx;
            border: none;
            border-radius: 20rpx;
            background-color: #FF4444;
            color: white;
            cursor: pointer;
            font-size: 24rpx;
            align-self: flex-start;
            transition: background-color 0.2s ease;
            z-index: 1;

            &:disabled {
              background-color: #ccc;
              cursor: not-allowed;
            }

            &:not(:disabled):active {
              background-color: #E83E3E;
            }
          }
        }
      }
    }
    
    /* 会员等级对应的商品列表 */
    .product-categories {
      display: flex;
      margin-bottom: 0;
      padding: 12rpx 16rpx;
      overflow-x: auto;
      border-bottom: 1rpx solid #eee;
      
      .category-item {
        padding: 8rpx 16rpx;
        margin-right: 8rpx;
        border: 1rpx solid #eee;
        border-radius: 20rpx;
        font-size: 24rpx;
        cursor: pointer;
        white-space: nowrap;
        background-color: #fafafa;
        transition: all 0.2s ease;

        &.active {
          background-color: #FF6B00;
          color: white;
          border-color: #FF6B00;
        }

        &:last-child {
          margin-right: 0;
        }
      }
    }
  }
}
</style>