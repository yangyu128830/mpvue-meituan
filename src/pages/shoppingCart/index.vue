<template>
  <div class="cart-container">
    <!-- Header -->
    <div class="cart-header">
      <div class="header-title">购物车</div>
      <div class="header-actions">
        <span 
          class="manage-btn" 
          :class="{ active: isManaging }"
          @click="toggleManage"
        >
          {{ isManaging ? '完成' : '管理' }}
        </span>
      </div>
    </div>

    <!-- Delivery info -->
    <div class="delivery-info" v-if="filteredItems.length > 0">
      <div class="delivery-address">
        <span class="icon-location">📍</span>
        <span class="address-text">北京市朝阳区建国路88号</span>
        <span class="arrow-right">></span>
      </div>
      <div class="delivery-time">
        <span class="icon-clock">⏰</span>
        <span class="time-text">预计30分钟送达</span>
      </div>
    </div>

    <!-- Shopping cart items list -->
    <scroll-view class="cart-items" :scroll-y="true">
      <!-- Item for shopping cart -->
      <div 
        class="cart-item" 
        v-for="(item, index) in filteredItems"
        :key="index"
      >
        <div class="item-selector">
          <label class="checkbox">
            <input 
              type="checkbox" 
              :checked="isItemSelected(item)"
              @change="toggleItemSelection(item)"
            >
            <i class="icon"></i>
          </label>
        </div>
        <div class="item-image">
          <img :src="item.image || 'https://via.placeholder.com/100x100'" mode="aspectFill">
        </div>
        <div class="item-details">
          <div class="item-name">{{ item.name }}</div>
          <div class="item-spec" v-if="item.spec">{{ item.spec }}</div>
          <div class="item-price">¥{{ item.price }}</div>
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
        <div class="empty-icon">🛒</div>
        <div class="empty-text">您的购物车是空的</div>
        <div class="empty-subtext">快去挑选喜欢的商品吧</div>
        <button class="go-shopping-btn" @click="goShopping">去逛逛</button>
      </div>
    </scroll-view>

    <!-- Bottom bar -->
    <div class="cart-footer" v-if="filteredItems.length > 0">
      <div class="footer-left">
        <label class="checkbox select-all">
          <input 
            type="checkbox" 
            :checked="isAllSelected"
            @change="toggleSelectAll"
          >
          <i class="icon"></i>
          <span>全选</span>
        </label>
        <div class="total-price">
          总计: <span class="price">¥{{ selectedTotalPrice }}</span>
        </div>
      </div>
      <div class="footer-right">
        <button 
          class="checkout-btn" 
          :class="{ disabled: selectedItems.length === 0 }"
          @click="checkoutSelected"
        >
          结算 ({{ selectedItems.length }})
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState, mapActions } from 'vuex'

export default {
  data() {
    return {
      isManaging: false
    }
  },
  computed: {
    ...mapState('shoppingCart', ['foods']),
    ...mapState('shoppingCart', ['selectedItems']),
    items() {
      // Flatten all items from categories that are in the cart (sequence > 0)
      return this.foods.reduce((acc, category) => {
        const cartItems = category.spus.filter(item => item.sequence > 0)
        // Add favorite and type properties to items if missing
        return acc.concat(cartItems.map(item => ({
          ...item,
          id: item.id || item.product_id,
          name: item.name,
          price: item.min_price || item.price,
          quantity: item.sequence || 1,
          description: item.description || '暂无描述',
          image: item.picture || 'https://via.placeholder.com/100x100',
          type: item.type || 'cart',
          isFavorite: item.isFavorite || false,
          spec: item.spec || ''
        })))
      }, [])
    },
    filteredItems() {
      // For now, show all items in cart
      return this.items
    },
    selectedItemsCount() {
      return this.selectedItems.length
    },
    selectedTotalPrice() {
      return this.filteredItems
        .filter(item => this.selectedItems.includes(item.id))
        .reduce((total, item) => {
          return total + (item.price * item.quantity)
        }, 0)
        .toFixed(2)
    },
    isAllSelected() {
      return this.selectedItems.length === this.filteredItems.length && this.filteredItems.length > 0
    }
  },
  methods: {
    ...mapActions('shoppingCart', ['addItemAction', 'reduceItemAction']),
    toggleManage() {
      this.isManaging = !this.isManaging
      if (!this.isManaging) {
        this.$store.commit('shoppingCart/changeSelectedItemsMut', [])
      }
    },
    isItemSelected(item) {
      return this.selectedItems.includes(item.id)
    },
    toggleItemSelection(item) {
      const newSelectedItems = [...this.selectedItems]
      const index = newSelectedItems.indexOf(item.id)
      if (index > -1) {
        newSelectedItems.splice(index, 1)
      } else {
        newSelectedItems.push(item.id)
      }
      this.$store.commit('shoppingCart/changeSelectedItemsMut', newSelectedItems)
    },
    toggleSelectAll() {
      let newSelectedItems = []
      if (!this.isAllSelected) {
        newSelectedItems = this.filteredItems.map(item => item.id)
      }
      this.$store.commit('shoppingCart/changeSelectedItemsMut', newSelectedItems)
    },
    increaseQuantity(item) {
      // Find the category and item index to dispatch the correct action
      for (let i = 0; i < this.foods.length; i++) {
        const category = this.foods[i]
        const itemIndex = category.spus.findIndex(spu => spu.id === item.id)
        if (itemIndex > -1) {
          this.addItemAction({ item, index: itemIndex })
          break
        }
      }
    },
    decreaseQuantity(item) {
      if (item.quantity > 1) {
        // Find the category and item index to dispatch the correct action
        for (let i = 0; i < this.foods.length; i++) {
          const category = this.foods[i]
          const itemIndex = category.spus.findIndex(spu => spu.id === item.id)
          if (itemIndex > -1) {
            this.reduceItemAction({ item, index: itemIndex })
            break
          }
        }
      } else {
        // If quantity is 1 and user clicks minus, remove item
        this.deleteItem(item)
      }
    },
    deleteItem(item) {
      // Find the category and item index to dispatch the correct action
      for (let i = 0; i < this.foods.length; i++) {
        const category = this.foods[i]
        const itemIndex = category.spus.findIndex(spu => spu.id === item.id)
        if (itemIndex > -1) {
          // Set quantity to 0
          category.spus[itemIndex].sequence = 0
          // Update category count and totalPrice
          category.count = category.spus.reduce((total, spu) => total + spu.sequence, 0)
          category.totalPrice = category.spus.reduce((total, spu) => total + (spu.min_price * spu.sequence), 0)
          // Commit changes to store
          this.$store.commit('shoppingCart/changeFoodsDataMut', this.foods)
          break
        }
      }
      // Remove from selected items
      this.$store.commit('shoppingCart/changeSelectedItemsMut', 
        this.selectedItems.filter(id => id !== item.id)
      )
    },
    deleteSelectedItems() {
      // Update foods in Vuex store
      const updatedFoods = this.foods.map(category => {
        // Set sequence to 0 for selected items
        const updatedSpus = category.spus.map(spu => {
          if (this.selectedItems.includes(spu.id)) {
            return { ...spu, sequence: 0 }
          }
          return spu
        })
        // Update category count and totalPrice
        const updatedCount = updatedSpus.reduce((total, spu) => total + spu.sequence, 0)
        const updatedTotalPrice = updatedSpus.reduce((total, spu) => total + (spu.min_price * spu.sequence), 0)
        return { ...category, spus: updatedSpus, count: updatedCount, totalPrice: updatedTotalPrice }
      })
      // Commit changes to store
      this.$store.commit('shoppingCart/changeFoodsDataMut', updatedFoods)
      // Clear selected items
      this.$store.commit('shoppingCart/changeSelectedItemsMut', [])
    },
    checkoutSelected() {
      if (this.selectedItems.length === 0) {
        wx.showToast({
          title: '请选择要结算的商品',
          icon: 'none'
        })
        return
      }
      wx.showToast({
        title: '结算成功',
        icon: 'success'
      })
      // Reset selected items
      this.selectedItems = []
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
.cart-container {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  background-color: #f5f5f5;
}

/* Header styles */
.cart-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #ff6700;
  color: white;
  padding: 20rpx 30rpx;
  
  .header-title {
    font-size: 36rpx;
    font-weight: bold;
  }
  
  .manage-btn {
    font-size: 28rpx;
    padding: 10rpx 20rpx;
    border-radius: 20rpx;
    background-color: rgba(255, 255, 255, 0.2);
    
    &.active {
      background-color: white;
      color: #ff6700;
    }
  }
}

/* Delivery info styles */
.delivery-info {
  background-color: white;
  margin: 10rpx;
  border-radius: 10rpx;
  padding: 20rpx;
  box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.05);
  
  .delivery-address {
    display: flex;
    align-items: center;
    margin-bottom: 15rpx;
    
    .icon-location {
      font-size: 24rpx;
      margin-right: 10rpx;
      color: #ff6700;
    }
    
    .address-text {
      flex: 1;
      font-size: 28rpx;
      color: #333;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
    }
    
    .arrow-right {
      font-size: 24rpx;
      color: #999;
    }
  }
  
  .delivery-time {
    display: flex;
    align-items: center;
    
    .icon-clock {
      font-size: 24rpx;
      margin-right: 10rpx;
      color: #ff6700;
    }
    
    .time-text {
      font-size: 26rpx;
      color: #666;
    }
  }
}

/* Items list styles */
.cart-items {
  flex: 1;
  padding-bottom: 100rpx;
}

.cart-item {
  background-color: white;
  margin: 10rpx;
  border-radius: 10rpx;
  padding: 20rpx;
  display: flex;
  align-items: center;
  box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.05);
  transition: all 0.3s ease;
  
  &:hover {
    transform: translateY(-2rpx);
    box-shadow: 0 4rpx 15rpx rgba(0, 0, 0, 0.1);
  }
}

.item-selector {
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
    width: 36rpx;
    height: 36rpx;
    border: 2rpx solid #ccc;
    border-radius: 50%;
    position: relative;
    background-color: white;
    
    &:hover {
      border-color: #ff6700;
    }
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
      font-size: 24rpx;
    }
  }
}

.item-image {
  width: 120rpx;
  height: 120rpx;
  border-radius: 10rpx;
  overflow: hidden;
  margin-right: 20rpx;
  
  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
}

.item-details {
  flex: 1;
  
  .item-name {
    font-size: 30rpx;
    font-weight: bold;
    color: #333;
    margin-bottom: 10rpx;
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
  }
  
  .item-spec {
    font-size: 24rpx;
    color: #999;
    margin-bottom: 15rpx;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
  
  .item-price {
    font-size: 28rpx;
    color: #ff6700;
    font-weight: bold;
  }
}

.item-actions {
  display: flex;
  align-items: center;
  
  .delete-btn {
    font-size: 24rpx;
    color: #ff6700;
    padding: 10rpx 15rpx;
    border: 1px solid #ff6700;
    border-radius: 20rpx;
    transition: all 0.3s ease;
    
    &:hover {
      background-color: #ff6700;
      color: white;
    }
  }
}

.quantity-control {
  display: flex;
  align-items: center;
  
  .btn-minus, .btn-plus {
    width: 44rpx;
    height: 44rpx;
    border-radius: 50%;
    border: 1px solid #ccc;
    background-color: white;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 24rpx;
    transition: all 0.3s ease;
    
    &:hover {
      border-color: #ff6700;
      color: #ff6700;
    }
  }
  
  .quantity {
    margin: 0 15rpx;
    font-size: 28rpx;
    color: #333;
    width: 40rpx;
    text-align: center;
  }
}

/* Empty state styles */
.empty-state {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 100rpx 0;
  
  .empty-icon {
    font-size: 120rpx;
    margin-bottom: 30rpx;
    color: #ccc;
  }
  
  .empty-text {
    font-size: 32rpx;
    color: #999;
    margin-bottom: 10rpx;
  }
  
  .empty-subtext {
    font-size: 24rpx;
    color: #bbb;
    margin-bottom: 50rpx;
  }
  
  .go-shopping-btn {
    background-color: #ff6700;
    color: white;
    border: none;
    padding: 15rpx 30rpx;
    border-radius: 30rpx;
    font-size: 28rpx;
    transition: all 0.3s ease;
    
    &:hover {
      background-color: #e55d00;
    }
  }
}

/* Bottom bar styles */
.cart-footer {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: white;
  padding: 20rpx 30rpx;
  border-top: 1px solid #eee;
  display: flex;
  justify-content: space-between;
  align-items: center;
  box-shadow: 0 -2rpx 10rpx rgba(0, 0, 0, 0.05);
  
  .footer-left {
    display: flex;
    align-items: center;
    
    .select-all {
      margin-right: 30rpx;
      font-size: 28rpx;
      color: #333;
    }
    
    .total-price {
      font-size: 28rpx;
      font-weight: bold;
      color: #333;
      
      .price {
        color: #ff6700;
        font-size: 32rpx;
        margin-left: 10rpx;
      }
    }
  }
  
  .footer-right {
    .checkout-btn {
      background-color: #ff6700;
      color: white;
      border: none;
      padding: 20rpx 40rpx;
      border-radius: 40rpx;
      font-size: 28rpx;
      transition: all 0.3s ease;
      
      &:hover {
        background-color: #e55d00;
      }
      
      &.disabled {
        background-color: #ccc;
        cursor: not-allowed;
        
        &:hover {
          background-color: #ccc;
        }
      }
    }
  }
}
</style>
