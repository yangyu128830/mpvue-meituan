<template>
  <div class="container">
    <div class="member-header">
      <div class="level-card" :class="levelClass">
        <h1>您的会员等级</h1>
        <div class="level-name">{{ levelName }}</div>
        <div class="points">当前积分: {{ points }}</div>
        <div class="progress">
          <div class="progress-bar" :style="{ width: progress + '%' }"></div>
        </div>
        <div class="next-level">距离{{ nextLevelName }}还差{{ nextLevelPoints - points }}积分</div>
      </div>
    </div>
    
    <!-- 阶段权益 -->
    <div class="stage-benefits">
      <h2>各等级权益</h2>
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
      <h2>福利商品</h2>
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
      <h2>商品优惠券</h2>
      <div class="coupon-list">
        <div class="coupon-item" v-for="coupon in productCoupons" :key="coupon.id">
          <div class="coupon-amount">{{ coupon.amount }}</div>
          <div class="coupon-desc">{{ coupon.desc }}</div>
          <div class="coupon-validity">有效期至: {{ coupon.validity }}</div>
          <button class="coupon-btn" :disabled="coupon.used">
            {{ coupon.used ? '已使用' : '立即领取' }}
          </button>
        </div>
      </div>
    </div>
    
    <!-- 会员等级对应的商品列表 -->
    <div class="member-products">
      <h2>会员专享商品</h2>
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
          { id: 1, icon: '🏨', title: '免费酒店', desc: '每年可免费入住任意酒店一次' }
        ]
      } else if (this.points >= 1000) {
        this.levelName = '黄金会员'
        this.levelClass = 'gold'
        this.benefits = [
          { id: 1, icon: '🎟️', title: '每月代金券', desc: '每月可领取100元代金券' }
        ]
      } else if (this.points >= 500) {
        this.levelName = '白银会员'
        this.levelClass = 'silver'
        this.benefits = [
          { id: 1, icon: '🎟️', title: '每月代金券', desc: '每月可领取50元代金券' }
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
    }
  }
}
</script>

<style lang="scss" scoped>
.container {
  padding: 20px;
}

.member-header {
  margin-bottom: 20px;
}

.level-card {
  padding: 20px;
  border-radius: 10px;
  color: white;
  text-align: center;

  &.normal {
    background-color: #ccc;
  }

  &.silver {
    background-color: #C0C0C0;
  }

  &.gold {
    background-color: #FFD700;
  }

  &.black-gold {
    background-color: #000;
  }

  .level-name {
    font-size: 24px;
    font-weight: bold;
    margin: 10px 0;
  }

  .points {
    font-size: 16px;
    margin-bottom: 10px;
  }

  .progress {
    height: 10px;
    background-color: rgba(255, 255, 255, 0.3);
    border-radius: 5px;
    margin: 10px 0;
    overflow: hidden;

    .progress-bar {
      height: 100%;
      background-color: white;
    }
  }

  .next-level {
    font-size: 14px;
  }
}

.member-benefits,
.member-rules {
  margin-bottom: 20px;
}

h2 {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 10px;
  padding-bottom: 5px;
  border-bottom: 1px solid #eee;
}

.benefit-list {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.benefit-item {
  display: flex;
  align-items: center;
  padding: 10px;
  border: 1px solid #eee;
  border-radius: 5px;

  .benefit-icon {
    font-size: 24px;
    margin-right: 10px;
  }

  .benefit-title {
    font-size: 16px;
    font-weight: bold;
    margin-bottom: 5px;
  }

  .benefit-desc {
    font-size: 14px;
    color: #666;
  }
}

.rule-list {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.rule-item {
  padding: 10px;
  border: 1px solid #eee;
  border-radius: 5px;

  h3 {
    font-size: 16px;
    font-weight: bold;
    margin-bottom: 5px;
  }

  p {
    font-size: 14px;
    color: #666;
    margin-bottom: 3px;
  }
}

/* 阶段权益样式 */
.stage-benefits {
  margin-bottom: 20px;
}

.stage-tabs {
  display: flex;
  margin-bottom: 10px;
  border-bottom: 1px solid #eee;
}

.stage-tab {
  padding: 10px 20px;
  cursor: pointer;
  font-size: 16px;
  color: #999;
  border-bottom: 2px solid transparent;

  &.active {
    color: #000;
    border-bottom-color: #000;
  }
}

.stage-content {
  padding: 20px;
  background-color: #f5f5f5;
  border-radius: 5px;
}

.stage-benefit-item {
  font-size: 16px;
  margin-bottom: 10px;
  padding-left: 15px;
  position: relative;

  &::before {
    content: '✓';
    position: absolute;
    left: 0;
    color: #4CAF50;
  }
}

/* 福利商品和会员专享商品样式 */
.welfare-products,
.member-products {
  margin-bottom: 20px;
}

.product-list {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 10px;
}

.product-item {
  display: flex;
  flex-direction: column;
  border: 1px solid #eee;
  border-radius: 5px;
  overflow: hidden;
}

.product-image {
  width: 100%;
  height: 100px;
  object-fit: cover;
}

.product-info {
  padding: 10px;
}

.product-name {
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 5px;
}

.product-price {
  display: flex;
  align-items: center;
  margin-bottom: 5px;

  .current-price {
    font-size: 16px;
    color: #ff4444;
    margin-right: 10px;
  }

  .original-price {
    font-size: 14px;
    color: #999;
    text-decoration: line-through;
  }
}

.product-desc {
  font-size: 14px;
  color: #666;
  margin-bottom: 5px;
}

.product-discount {
  font-size: 14px;
  color: #ff4444;
}

/* 商品优惠券样式 */
.product-coupons {
  margin-bottom: 20px;
}

.coupon-list {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 10px;
}

.coupon-item {
  display: flex;
  flex-direction: column;
  padding: 15px;
  border: 1px solid #eee;
  border-radius: 5px;
  position: relative;
}

.coupon-amount {
  font-size: 20px;
  font-weight: bold;
  color: #ff4444;
  margin-bottom: 5px;
}

.coupon-desc {
  font-size: 14px;
  color: #666;
  margin-bottom: 5px;
}

.coupon-validity {
  font-size: 12px;
  color: #999;
  margin-bottom: 10px;
}

.coupon-btn {
  padding: 5px 10px;
  border: none;
  border-radius: 3px;
  background-color: #ff4444;
  color: white;
  cursor: pointer;
  font-size: 14px;
  align-self: flex-start;

  &:disabled {
    background-color: #ccc;
    cursor: not-allowed;
  }
}

/* 商品分类样式 */
.product-categories {
  display: flex;
  margin-bottom: 10px;
  overflow-x: auto;
}

.category-item {
  padding: 8px 16px;
  margin-right: 10px;
  border: 1px solid #eee;
  border-radius: 20px;
  font-size: 14px;
  cursor: pointer;
  white-space: nowrap;

  &.active {
    background-color: #ff4444;
    color: white;
    border-color: #ff4444;
  }
}
</style>