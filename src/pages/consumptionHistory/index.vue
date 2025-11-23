<template>
  <div class="container">
    <div class="header-c">
      <button @click="goBack">返回</button>
      <h2>消费记录</h2>
    </div>
    <div class="total-consumption" v-if="orders.length > 0">
      <span class="label">共消费：</span>
      <span class="amount">¥{{ totalConsumption }}</span>
    </div>
    <div class="order-list" v-if="orders.length > 0">
      <div class="order-item" v-for="(order, index) in orders" :key="index">
        <div class="order-header">
          <span class="order-number">订单号：{{ order.number }}</span>
          <span class="order-date">{{ order.date }}</span>
        </div>
        <div class="order-detail">
          <div class="order-item-info">
            <img :src="order.image" alt="" class="item-image">
            <div class="item-info">
              <div class="item-name">{{ order.name }}</div>
              <div class="item-price">¥{{ order.price }} x {{ order.quantity }}</div>
            </div>
          </div>
          <div class="order-total">¥{{ order.amount }}</div>
        </div>
      </div>
    </div>
    <div class="empty-state" v-else>
      <img src="https://cdn.pixabay.com/photo/2016/11/01/14/58/nurse-1789400_960_720.png" alt="无订单">
      <p>暂无消费记录</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      orders: []
    }
  },
  computed: {
    totalConsumption() {
      return this.orders.reduce((sum, order) => sum + order.amount, 0).toFixed(2)
    }
  },
  methods: {
    goBack() {
      wx.navigateBack()
    }
  },
  onLoad() {
    // Get orders from local storage, or generate sample data if none
    let orders = wx.getStorageSync('orders')
    if (!orders || orders.length === 0) {
      // Generate sample data
      orders = [
        {
          number: 'MT202310010001',
          date: '2023-10-01',
          name: '超级牛肉汉堡套餐',
          price: 39.9,
          quantity: 1,
          amount: 39.9,
          image: 'https://cdn.pixabay.com/photo/2016/11/01/14/58/nurse-1789400_960_720.png'
        },
        {
          number: 'MT202310020001',
          date: '2023-10-02',
          name: '经典鸡肉卷套餐',
          price: 29.9,
          quantity: 2,
          amount: 59.8,
          image: 'https://cdn.pixabay.com/photo/2016/11/01/14/58/nurse-1789400_960_720.png'
        },
        {
          number: 'MT202310030001',
          date: '2023-10-03',
          name: '全家桶套餐',
          price: 129.9,
          quantity: 1,
          amount: 129.9,
          image: 'https://cdn.pixabay.com/photo/2016/11/01/14/58/nurse-1789400_960_720.png'
        }
      ]
      // Save sample data to local storage
      wx.setStorageSync('orders', orders)
    }
    this.orders = orders
  }
}
</script>

<style lang="scss" scoped>
.container {
  .header-c {
    display: flex;
    align-items: center;
    height: 80rpx;
    background-color: white;
    padding: 0 20rpx;
    border-bottom: 2rpx solid #f0f0f0;
    
    button {
      background: none;
      border: none;
      font-size: 28rpx;
      color: #333;
    }
    
    h2 {
      flex: 1;
      text-align: center;
      font-size: 32rpx;
      color: #333;
      margin: 0;
    }
  }
  
  .total-consumption {
    background-color: #FFD26B;
    color: white;
    padding: 20rpx;
    font-size: 32rpx;
    text-align: center;
    
    .amount {
      font-weight: bold;
      font-size: 36rpx;
    }
  }
  
  .order-list {
    background-color: white;
    margin-top: 20rpx;
    
    .order-item {
      border-bottom: 2rpx solid #f0f0f0;
      padding: 20rpx;
      
      &:last-child {
        border-bottom: none;
      }
      
      .order-header {
        display: flex;
        justify-content: space-between;
        margin-bottom: 10rpx;
        
        .order-number {
          font-size: 24rpx;
          color: #666;
        }
        
        .order-date {
          font-size: 24rpx;
          color: #666;
        }
      }
      
      .order-detail {
        display: flex;
        justify-content: space-between;
        align-items: center;
        
        .order-item-info {
          display: flex;
          align-items: center;
          
          .item-image {
            width: 100rpx;
            height: 100rpx;
            border-radius: 10rpx;
            margin-right: 20rpx;
          }
          
          .item-info {
            .item-name {
              font-size: 28rpx;
              color: #333;
              margin-bottom: 5rpx;
            }
            
            .item-price {
              font-size: 24rpx;
              color: #666;
            }
          }
        }
        
        .order-total {
          font-size: 28rpx;
          color: #FF6B00;
          font-weight: bold;
        }
      }
    }
  }
  
  .empty-state {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 60vh;
    
    img {
      width: 200rpx;
      height: 200rpx;
      margin-bottom: 20rpx;
    }
    
    p {
      font-size: 28rpx;
      color: #666;
    }
  }
}
</style>