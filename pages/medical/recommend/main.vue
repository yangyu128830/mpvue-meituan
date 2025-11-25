<template>
  <div class="recommend-page">
    <!-- Basic Medicine Recommendations -->
    <div class="medicine-section">
      <div class="section">
        <div class="l"></div>
        <span class="m">基础药品推荐</span>
        <div class="r"></div>
      </div>
      <div class="medicine-list">
        <div class="medicine-item" v-for="item in basicMedicines" :key="item.id">
          <img :src="item.image" alt="{{ item.name }}" />
          <div class="medicine-info">
            <h4>{{ item.name }}</h4>
            <p>{{ item.description }}</p>
            <span class="price">¥{{ item.price }}</span>
            <button class="add-to-cart" @click="addToCart(item)">加入购物车</button>
          </div>
        </div>
      </div>
    </div>

    <!-- Frequently Bought Medicines -->
    <div class="medicine-section">
      <div class="section">
        <div class="l"></div>
        <span class="m">常买药品</span>
        <div class="r"></div>
      </div>
      <div class="medicine-list">
        <div class="medicine-item" v-for="item in frequentMedicines" :key="item.id">
          <img :src="item.image" alt="{{ item.name }}" />
          <div class="medicine-info">
            <h4>{{ item.name }}</h4>
            <p>{{ item.description }}</p>
            <span class="price">¥{{ item.price }}</span>
            <button class="add-to-cart" @click="addToCart(item)">加入购物车</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import store from '@/store'

export default {
  data() {
    return {
      basicMedicines: [
        { id: 1, name: '创可贴', description: '止血、防感染', price: 5.9, image: '/static/images/icon_medicine.svg' },
        { id: 2, name: '碘伏消毒液', description: '伤口消毒', price: 8.5, image: '/static/images/icon_medicine.svg' },
        { id: 3, name: '体温计', description: '测量体温', price: 22.8, image: '/static/images/icon_medicine.svg' },
        { id: 4, name: '感冒灵颗粒', description: '缓解感冒症状', price: 16.9, image: '/static/images/icon_medicine.svg' }
      ],
      frequentMedicines: [
        { id: 1, name: '维生素C片', description: '补充维生素C', price: 19.9, image: '/static/images/icon_medicine.svg' },
        { id: 2, name: '钙片', description: '补钙', price: 45.6, image: '/static/images/icon_medicine.svg' },
        { id: 3, name: '润喉糖', description: '缓解喉咙不适', price: 12.3, image: '/static/images/icon_medicine.svg' },
        { id: 4, name: '眼药水', description: '缓解眼疲劳', price: 28.7, image: '/static/images/icon_medicine.svg' }
      ]
    }
  },
  methods: {
    addToCart(item) {
      // Initialize medical shop if not exists
      let medicalShop = store.state.shoppingCart.shopInfo
      if (!medicalShop || medicalShop.name !== '医疗商城') {
        medicalShop = {
          name: '医疗商城',
          prompt_text: '',
          activity_info: [],
          selectedArr: []
        }
        const foods = [
          {
            name: '推荐药品',
            count: 0,
            totalPrice: 0,
            spus: []
          }
        ]
        store.commit('shoppingCart/changeShopInfoDataMut', medicalShop)
        store.commit('shoppingCart/changeFoodsDataMut', foods)
        const spus = {
          title: '推荐药品',
          index: 0,
          list: []
        }
        store.commit('shoppingCart/changeSpusDataMut', spus)
      }

      const spus = store.state.shoppingCart.spus
      const existingIndex = spus.list.findIndex(spu => spu.name === item.name)
      
      if (existingIndex !== -1) {
        // Increment quantity for existing item
        store.dispatch('shoppingCart/addItemAction', { item: spus.list[existingIndex], index: existingIndex })
      } else {
        // Add new item to cart
        const newSpu = {
          name: item.name,
          min_price: item.price,
          sequence: 1,
          image: item.image,
          description: item.description
        }
        spus.list.push(newSpu)
        store.commit('shoppingCart/changeSpusDataMut', spus)
        
        // Update food count and total price
        const foods = store.state.shoppingCart.foods
        foods[0].count += 1
        foods[0].totalPrice += item.price + (item.price > 0 ? 1 : 0)
        store.commit('shoppingCart/changeFoodsDataMut', foods)
      }
    }
  }
}
</script>

<style lang="scss" scoped>
@import "@/assets/global.scss";

.recommend-page {
  padding: 0;
}

.medicine-section {
  margin-bottom: 40rpx;
}

.section {
  display: flex;
  align-items: center;
  margin-bottom: 20rpx;
  
  .l, .r {
    flex: 1;
    height: 2rpx;
    background-color: $spLine-color;
  }
  
  .m {
    font-size: 28rpx;
    color: $textBlack-color;
    margin: 0 20rpx;
  }
}

.medicine-list {
  display: flex;
  flex-wrap: wrap;
  gap: 20rpx;
}

.medicine-item {
  width: calc(50% - 10rpx);
  background-color: white;
  border-radius: 8rpx;
  padding: 20rpx;
  box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.05);
  
  img {
    width: 100%;
    height: 160rpx;
    object-fit: contain;
    border-radius: 8rpx;
    margin-bottom: 10rpx;
  }

  .medicine-info {
    h4 {
      font-size: 28rpx;
      margin-bottom: 10rpx;
      color: $textBlack-color;
    }

    p {
      font-size: 24rpx;
      color: $textDarkGray-color;
      margin-bottom: 10rpx;
      line-height: 1.4;
    }

    .price {
      font-size: 32rpx;
      color: $mtRed-color;
      font-weight: bold;
      display: block;
      margin-bottom: 10rpx;
    }

    .add-to-cart {
      width: 100%;
      padding: 16rpx 0;
      background-color: $theme-color;
      color: white;
      border: none;
      border-radius: 30rpx;
      cursor: pointer;
      font-size: 24rpx;
    }
  }
}
</style>