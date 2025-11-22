<template>
  <div class="container">
    <!-- Header with tabs and management button -->
    <div class="header">
      <div class="tabs">
        <span 
          class="tab-item" 
          :class="{ active: activeTab === 'all' }"
          @click="switchTab('all')"
        >
          全部
        </span>
        <span 
          class="tab-item" 
          :class="{ active: activeTab === 'favorite' }"
          @click="switchTab('favorite')"
        >
          收藏
        </span>
        <span 
          class="tab-item" 
          :class="{ active: activeTab === 'frequent' }"
          @click="switchTab('frequent')"
        >
          常购
        </span>
      </div>
      <div class="manage-btn" @click="toggleManage">
        {{ isManaging ? '完成' : '管理' }}
      </div>
    </div>

    <!-- Shopping cart items list -->
    <scroll-view class="items-list" :scroll-y="true">
      <!-- Item for shopping cart -->
      <div 
        class="item-card" 
        v-for="(item, index) in filteredItems"
        :key="index"
      >
        <div class="item-checkbox" v-if="isManaging">
          <label class="checkbox">
            <input 
              type="checkbox" 
              :checked="isItemSelected(item)"
              @change="toggleItemSelection(item)"
            >
            <i class="icon"></i>
          </label>
        </div>
        <div class="item-info">
          <img class="item-image" :src="item.image" mode="aspectFill">
          <div class="item-details">
            <div class="item-name">{{ item.name }}</div>
            <div class="item-price">¥{{ item.price }}</div>
            <div class="item-desc">{{ item.description }}</div>
          </div>
        </div>
        <div class="item-actions">
          <span 
            class="delete-btn" 
            v-if="isManaging"
            @click="deleteItem(item)"
          >
            删除
          </span>
          <div class="quantity-control" v-else>
            <button class="btn-minus" @click="decreaseQuantity(item)">-</button>
            <span class="quantity">{{ item.quantity }}</span>
            <button class="btn-plus" @click="increaseQuantity(item)">+</button>
          </div>
        </div>
      </div>

      <!-- Empty state -->
      <div class="empty-state" v-if="filteredItems.length === 0">
        <img class="empty-icon" src="/static/images/empty_cart.png" alt="Empty cart">
        <div class="empty-text">您的购物车是空的</div>
        <button class="go-shopping-btn" @click="goShopping">去逛逛</button>
      </div>
    </scroll-view>

    <!-- Bottom bar for batch operations -->
    <div class="bottom-bar" v-if="isManaging && filteredItems.length > 0">
      <div class="select-all">
        <label class="checkbox">
          <input 
            type="checkbox" 
            :checked="isAllSelected"
            @change="toggleSelectAll"
          >
          <i class="icon"></i>
          <span>全选</span>
        </label>
      </div>
      <div class="batch-actions">
  <button class="delete-all-btn" @click="deleteSelectedItems">删除选中</button>
  <button class="favorite-all-btn" @click="toggleFavoriteSelectedItems">批量收藏</button>
  <button class="checkout-btn" @click="checkoutSelectedItems">结算</button>
</div>
    </div>

    <!-- Bottom bar for normal state -->
    <div class="bottom-bar" v-else-if="filteredItems.length > 0">
      <div class="total-price">
        总计: <span class="price">¥{{ totalPrice }}</span>
      </div>
      <button class="checkout-btn" @click="checkoutAll">结算</button>
    </div>
  </div>
</template>

<script>
import { mapState, mapActions } from 'vuex'

export default {
  data() {
    return {
      activeTab: 'all',
      isManaging: false,
      selectedItems: [],
      emptyImage: 'https://picsum.photos/id/1/200/200'
    }
  },
  computed: {
     ...mapState('shoppingCart', ['foods']),
     items() {
       // Flatten all items from categories that are in the cart (sequence > 0)
       return this.foods.reduce((acc, category) => {
         const cartItems = category.list.filter(item => item.sequence > 0)
         // Add favorite and type properties to items if missing
         return acc.concat(cartItems.map(item => ({
           ...item,
           id: item.id || item.product_id,
           name: item.name,
           price: item.min_price || item.price,
           quantity: item.sequence || 1,
           description: item.description || '暂无描述',
           image: item.picture || 'https://picsum.photos/id/1/200/200',
           type: item.type || 'cart',
           isFavorite: item.isFavorite || false
         })))
       }, [])
     },
    filteredItems() {
      switch (this.activeTab) {
        case 'favorite':
          return this.items.filter(item => item.isFavorite)
        case 'frequent':
          return this.items.filter(item => item.type === 'frequent')
        default:
          return this.items.filter(item => item.type !== 'favorite' || item.isFavorite)
      }
    },
    totalPrice() {
      return this.filteredItems.reduce((total, item) => {
        return total + (item.price * item.quantity)
      }, 0)
    },
    isAllSelected() {
      return this.selectedItems.length === this.filteredItems.length && this.filteredItems.length > 0
    }
  },
  methods: {
    switchTab(tab) {
      this.activeTab = tab
    },
    toggleManage() {
      this.isManaging = !this.isManaging
      if (!this.isManaging) {
        this.selectedItems = []
      }
    },
    isItemSelected(item) {
      return this.selectedItems.includes(item.id)
    },
    toggleItemSelection(item) {
      const index = this.selectedItems.indexOf(item.id)
      if (index > -1) {
        this.selectedItems.splice(index, 1)
      } else {
        this.selectedItems.push(item.id)
      }
    },
    toggleSelectAll() {
      if (this.isAllSelected) {
        this.selectedItems = []
      } else {
        this.selectedItems = this.filteredItems.map(item => item.id)
      }
    },
    increaseQuantity(item) {
      item.quantity++
    },
    decreaseQuantity(item) {
      if (item.quantity > 1) {
        item.quantity--
      }
    },
    deleteItem(item) {
      const index = this.items.findIndex(i => i.id === item.id)
      if (index > -1) {
        this.items.splice(index, 1)
      }
      this.removeFromSelected(item.id)
    },
    deleteSelectedItems() {
      this.items = this.items.filter(item => !this.selectedItems.includes(item.id))
      this.selectedItems = []
    },
    checkoutAll() {
      wx.showToast({
        title: '结算成功',
        icon: 'success'
      })
    },
    checkoutSelectedItems() {
  wx.showToast({
    title: '结算成功',
    icon: 'success'
  })
},
toggleFavoriteSelectedItems() {
  this.items.forEach(item => {
    if (this.selectedItems.includes(item.id)) {
      item.isFavorite = !item.isFavorite
    }
  })
},
    goShopping() {
      wx.switchTab({
        url: '/pages/home/main'
      })
    },
    removeFromSelected(id) {
      const index = this.selectedItems.indexOf(id)
      if (index > -1) {
        this.selectedItems.splice(index, 1)
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.container {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  background-color: #f5f5f5;
}

/* Header styles */
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: white;
  padding: 20rpx 30rpx;
  border-bottom: 1px solid #eee;
}

.tabs {
  display: flex;
}

.tab-item {
  padding: 15rpx 30rpx;
  font-size: 32rpx;
  color: #666;
  position: relative;
  margin-right: 20rpx;
  
  &.active {
    color: #ff6700;
    font-weight: bold;
    
    &::after {
      content: '';
      position: absolute;
      bottom: -20rpx;
      left: 0;
      width: 100%;
      height: 4rpx;
      background-color: #ff6700;
    }
  }
}

.manage-btn {
  font-size: 28rpx;
  color: #ff6700;
}

/* Items list styles */
.items-list {
  flex: 1;
}

.item-card {
  background-color: white;
  margin: 10rpx 0;
  padding: 20rpx;
  display: flex;
  align-items: center;
}

.item-checkbox {
  margin-right: 20rpx;
}

.checkbox {
  display: flex;
  align-items: center;
  font-size: 28rpx;
  
  input {
    display: none;
  }
  
  .icon {
    width: 30rpx;
    height: 30rpx;
    border: 2rpx solid #ccc;
    border-radius: 50%;
    position: relative;
    margin-right: 10rpx;
  }
  
  input:checked + .icon {
    background-color: #ff6700;
    border-color: #ff6700;
    
    &::after {
      content: '✓';
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      color: white;
      font-size: 20rpx;
    }
  }
}

.item-info {
  display: flex;
  flex: 1;
}

.item-image {
  width: 150rpx;
  height: 150rpx;
  border-radius: 10rpx;
  margin-right: 20rpx;
}

.item-details {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.item-name {
  font-size: 32rpx;
  font-weight: bold;
  margin-bottom: 10rpx;
}

.item-price {
  font-size: 28rpx;
  color: #ff6700;
  margin-bottom: 10rpx;
}

.item-desc {
  font-size: 24rpx;
  color: #999;
}

.item-actions {
  display: flex;
  align-items: center;
}

.delete-btn {
  font-size: 28rpx;
  color: #ff6700;
}

.quantity-control {
  display: flex;
  align-items: center;
}

.btn-minus, .btn-plus {
  width: 40rpx;
  height: 40rpx;
  border-radius: 50%;
  border: 1px solid #ccc;
  background-color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 24rpx;
}

.quantity {
  margin: 0 10rpx;
  font-size: 28rpx;
}

/* Empty state styles */
.empty-state {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 100rpx 0;
}

.empty-icon {
  width: 200rpx;
  height: 200rpx;
  margin-bottom: 30rpx;
  opacity: 0.5;
}

.empty-text {
  font-size: 28rpx;
  color: #999;
  margin-bottom: 30rpx;
}

.go-shopping-btn {
  background-color: #ff6700;
  color: white;
  border: none;
  padding: 15rpx 30rpx;
  border-radius: 30rpx;
  font-size: 28rpx;
}

/* Bottom bar styles */
.bottom-bar {
  background-color: white;
  padding: 20rpx 30rpx;
  border-top: 1px solid #eee;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.select-all {
  display: flex;
  align-items: center;
}

.batch-actions {
  display: flex;
}

.delete-all-btn {
  background-color: white;
  color: #ff6700;
  border: 1px solid #ff6700;
  padding: 10rpx 20rpx;
  border-radius: 20rpx;
  font-size: 24rpx;
  margin-right: 20rpx;
}

.favorite-all-btn {
  background-color: white;
  color: #ff6700;
  border: 1px solid #ff6700;
  padding: 10rpx 20rpx;
  border-radius: 20rpx;
  font-size: 24rpx;
  margin-right: 20rpx;
}

.checkout-btn {
  background-color: #ff6700;
  color: white;
  border: none;
  padding: 15rpx 30rpx;
  border-radius: 30rpx;
  font-size: 28rpx;
}

.total-price {
  font-size: 28rpx;
  font-weight: bold;
}

.price {
  color: #ff6700;
  font-size: 32rpx;
}
</style>
