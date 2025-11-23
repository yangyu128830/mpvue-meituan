<template>
  <div class="container">
    <!-- User Info Header -->
    <div class="header-c">
      <div class="avatar-c">
        <img src="https://wx.qlogo.cn/mmopen/vi_32/Q0j4TwGTfTJvoc0l3Oe4lWTMtUvLd7UYm9IvSWNjM6S5ibZBu3OE5XbCEqgPw9llpibmkyqEX9GbLKOCfTIe6wWQ/132" alt="User Avatar" class="avatar">
        <div class="avatar-badge" @click="viewMemberLevel">{{ memberLevel }}</div>
      </div>
      <div class="info-c">
        <div class="name-row">
          <span class="name" @click="editNickname">{{ nickname }}</span>
          <i class="icon mt-edit-o edit-icon" @click="editNickname"></i>
        </div>
        <span class="phone">15214313256</span>
        <div class="address-row" @click="editAddress">
          <i class="icon mt-my-location-o"></i>
          <span class="address">{{ address }}</span>
          <i class="icon mt-arrow-right-o"></i>
        </div>
      </div>
    </div>
    
    <!-- Quick Actions -->
    <div class="quick-actions">
      <div class="action-item" @click="itemClick({ path: '/pages/redPacket/main' })">
        <i class="icon mt-red-packet-o action-icon"></i>
        <span class="action-text">红包</span>
        <span class="action-badge">{{ (itemList.find(i => i.title === '美团红包') || {}).amount || 0 }}</span>
      </div>
      <div class="action-item" @click="itemClick({ path: '/pages/couponList/main' })">
        <i class="icon mt-coupon-o action-icon"></i>
        <span class="action-text">代金券</span>
        <span class="action-badge">{{ (itemList.find(i => i.title === '商家代金券') || {}).amount || 0 }}</span>
      </div>
      <div class="action-item" @click="itemClick({ path: '/pages/orderList/main?status=1' })">
        <i class="icon mt-clock-o action-icon"></i>
        <span class="action-text">待收货</span>
        <span class="action-badge">2</span>
      </div>
      <div class="action-item" @click="itemClick({ path: '/pages/orderList/main?status=0' })">
        <i class="icon mt-file-text-o action-icon"></i>
        <span class="action-text">全部订单</span>
      </div>
    </div>
    
    <!-- Main Menu -->
    <div class="menu-section">
      <div class="menu-title">常用功能</div>
      <div class="menu-list">
        <div class="menu-item" v-for="(item, index) in menuItems" :key="index" @click="itemClick(item)">
          <i class='icon menu-icon' :class="item.icon"></i>
          <span class="menu-text">{{item.title}}</span>
          <i class='icon mt-arrow-right-o menu-arrow'></i>
        </div>
      </div>
    </div>
    
    <!-- Help & Support -->
    <div class="menu-section">
      <div class="menu-title">帮助与支持</div>
      <div class="menu-list">
        <div class="menu-item" v-for="(item, index) in helpItems" :key="index" @click="itemClick(item)">
          <i class='icon menu-icon' :class="item.icon"></i>
          <span class="menu-text">{{item.title}}</span>
          <i class='icon mt-arrow-right-o menu-arrow'></i>
        </div>
      </div>
    </div>
    
    <!-- Logout Button -->
    <div class="logout-btn" @click="logoutClick">退出账号</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      nickname: '光强',
      address: '北京市海淀区中关村',
      memberLevel: '黄金会员',
      itemList: [
        {
          title: '美团红包',
          icon: 'mt-red-packet-o',
          path: '/pages/redPacket/main',
          amount: 4
        }, 
        {
          title: '商家代金券',
          icon: 'mt-coupon-o',
          path: '/pages/couponList/main',
          amount: 10
        },
        {
          title: '我的地址',
          icon: 'mt-my-location-o',
          path: '/pages/addressList/main'
        },
        {
          title: '修改密码',
          icon: 'mt-lock-o',
          path: '/pages/editPassword/main'
        },
        {
          title: '消费记录',
          icon: 'mt-file-text-o',
          path: '/pages/consumptionHistory/main'
        },
        {
          title: '最近访问',
          icon: 'mt-eye-o',
          path: '/pages/recentVisits/main'
        },
        {
          title: '邀请有奖',
          icon: 'mt-gift-o'
        },
        {
          title: '客服中心',
          icon: 'mt-customer-service-o'
        },
        {
          title: '帮助和反馈',
          icon: 'mt-help-o',
          path: '/pages/feedback/main'
        },
        {
          title: '协议和说明',
          icon: 'mt-protocol-o',
          path: '/pages/protocol/main'
        }
      ],
      // Split items into two categories
      menuItems: [
        {
          title: '我的地址',
          icon: 'mt-my-location-o',
          path: '/pages/addressList/main'
        },
        {
          title: '修改密码',
          icon: 'mt-lock-o',
          path: '/pages/editPassword/main'
        },
        {
          title: '消费记录',
          icon: 'mt-file-text-o',
          path: '/pages/consumptionHistory/main'
        },
        {
          title: '最近访问',
          icon: 'mt-eye-o',
          path: '/pages/recentVisits/main'
        },
        {
          title: '邀请有奖',
          icon: 'mt-gift-o'
        }
      ],
      helpItems: [
        {
          title: '客服中心',
          icon: 'mt-customer-service-o'
        },
        {
          title: '帮助和反馈',
          icon: 'mt-help-o',
          path: '/pages/feedback/main'
        },
        {
          title: '协议和说明',
          icon: 'mt-protocol-o',
          path: '/pages/protocol/main'
        }
      ]
    }
  },
  computed: {

  },
  methods: {
    itemClick(e) {
      wx.navigateTo({url: e.path})
    },
    editNickname() {
      wx.navigateTo({url: '/pages/editNickname/main'})
    },
    editAddress() {
      wx.navigateTo({url: '/pages/addressList/main'})
    },
    viewMemberLevel() {
      wx.navigateTo({url: '/pages/memberLevel/main'})
    },
    logoutClick() {
      wx.showModal({
          title: '确认退出？',
          content: '退出登录后将无法查看订单，重新登录即可查看',
          confirmColor: '#FFC24A',
          success: function(res) {
            if (res.confirm) {
              // Perform logout actions here
            }
        }
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.container {
  background-color: #F5F5F5;
  min-height: 100vh;
  
  .header-c {
    display: flex;
    align-items: center;
    padding: 30rpx 20rpx;
    background-color: #FFD26B;
    
    .avatar-c {
      position: relative;
      
      .avatar {
        width: 120rpx;
        height: 120rpx;
        border-radius: 60rpx;
        border: 4rpx solid white;
      }
      
      .avatar-badge {
        position: absolute;
        bottom: -5rpx;
        right: -5rpx;
        background-color: #FF6B00;
        color: white;
        font-size: 20rpx;
        padding: 4rpx 10rpx;
        border-radius: 12rpx;
        box-shadow: 0 2rpx 4rpx rgba(0,0,0,0.1);
      }
    }
    
    .info-c {
      display: flex;
      flex-direction: column;
      margin-left: 30rpx;
      flex: 1;
      
      .name-row {
        display: flex;
        align-items: center;
        margin-bottom: 8rpx;
        
        .name {
          font-size: 32rpx;
          color: $textBlack-color;
          font-weight: bold;
          margin-right: 10rpx;
        }
        
        .edit-icon {
          font-size: 24rpx;
          color: rgba(0,0,0,0.6);
        }
      }
      
      .phone {
        font-size: 28rpx;
        color: $textBlack-color;
        margin-bottom: 8rpx;
      }
      
      .address-row {
        display: flex;
        align-items: center;
        background-color: rgba(255,255,255,0.3);
        padding: 6rpx 12rpx;
        border-radius: 16rpx;
        
        i {
          font-size: 22rpx;
          color: $textBlack-color;
          margin-right: 8rpx;
        }
        
        .address {
          font-size: 24rpx;
          color: $textBlack-color;
          flex: 1;
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
        }
        
        i:last-child {
          font-size: 20rpx;
          color: rgba(0,0,0,0.4);
          margin-right: 0;
        }
      }
    }
  }
  
  // Quick Actions
  .quick-actions {
    display: flex;
    background-color: white;
    margin: 20rpx;
    border-radius: 12rpx;
    padding: 20rpx 0;
    box-shadow: 0 2rpx 8rpx rgba(0,0,0,0.05);
    
    .action-item {
      flex: 1;
      display: flex;
      flex-direction: column;
      align-items: center;
      
      .action-icon {
        font-size: 44rpx;
        color: #FF6B00;
        margin-bottom: 8rpx;
      }
      
      .action-text {
        font-size: 24rpx;
        color: $textBlack-color;
        margin-bottom: 4rpx;
      }
      
      .action-badge {
        background-color: #FF6B00;
        color: white;
        font-size: 18rpx;
        padding: 2rpx 8rpx;
        border-radius: 10rpx;
        min-width: 30rpx;
        text-align: center;
      }
    }
  }
  
  // Menu Sections
  .menu-section {
    background-color: white;
    margin: 0 20rpx 20rpx;
    border-radius: 12rpx;
    box-shadow: 0 2rpx 8rpx rgba(0,0,0,0.05);
    
    .menu-title {
      font-size: 28rpx;
      color: $textBlack-color;
      font-weight: bold;
      padding: 20rpx 24rpx;
      border-bottom: 2rpx solid #F5F5F5;
    }
    
    .menu-list {
      
      .menu-item {
        display: flex;
        align-items: center;
        padding: 24rpx;
        border-bottom: 2rpx solid #F5F5F5;
        
        &:last-child {
          border-bottom: none;
        }
        
        .menu-icon {
          font-size: 36rpx;
          color: #FF6B00;
          margin-right: 24rpx;
          width: 40rpx;
          text-align: center;
        }
        
        .menu-text {
          font-size: 28rpx;
          color: $textBlack-color;
          flex: 1;
        }
        
        .menu-arrow {
          font-size: 24rpx;
          color: $textGray-color;
        }
      }
    }
  }
  
  // Logout Button
  .logout-btn {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 88rpx;
    background-color: white;
    margin: 20rpx;
    border-radius: 12rpx;
    color: #FF6B00;
    font-size: 28rpx;
    box-shadow: 0 2rpx 8rpx rgba(0,0,0,0.05);
  }
}
</style>
