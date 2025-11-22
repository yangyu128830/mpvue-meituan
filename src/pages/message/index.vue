<template>
  <div class="message-page">
    <!-- Top navigation with three sections -->
    <div class="top-nav">
      <div class="tabs">
        <div class="tab" :class="{active: activeTab === 'order'}" @click="activeTab = 'order'">订单动态</div>
        <div class="tab" :class="{active: activeTab === 'welfare'}" @click="activeTab = 'welfare'">粉丝福利</div>
        <div class="tab" :class="{active: activeTab === 'service'}" @click="activeTab = 'service'">服务提醒</div>
      </div>
      <div class="right-buttons">
        <button class="btn" @click="clearMessages">清理</button>
        <button class="btn" @click="managePermissions">信息管理</button>
      </div>
    </div>

    <!-- Message content area -->
    <div class="message-content">
      <!-- Order dynamic messages -->
      <div v-if="activeTab === 'order'" class="message-list">
        <div class="message-item" v-for="(item, index) in orderMessages" :key="index">
          <div class="avatar">
            <img :src="item.avatar" alt="">
          </div>
          <div class="message-info">
            <div class="title">{{ item.title }}</div>
            <div class="content">{{ item.content }}</div>
            <div class="time">{{ item.time }}</div>
          </div>
        </div>
      </div>

      <!-- Welfare messages -->
      <div v-if="activeTab === 'welfare'" class="message-list">
        <div class="message-item" v-for="(item, index) in welfareMessages" :key="index">
          <div class="avatar">
            <img :src="item.avatar" alt="">
          </div>
          <div class="message-info">
            <div class="title">{{ item.title }}</div>
            <div class="content">{{ item.content }}</div>
            <div class="time">{{ item.time }}</div>
          </div>
        </div>
      </div>

      <!-- Service messages -->
      <div v-if="activeTab === 'service'" class="message-list">
        <div class="message-item" v-for="(item, index) in serviceMessages" :key="index">
          <div class="avatar">
            <img :src="item.avatar" alt="">
          </div>
          <div class="message-info">
            <div class="title">{{ item.title }}</div>
            <div class="content">{{ item.content }}</div>
            <div class="time">{{ item.time }}</div>
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
      activeTab: 'order',
      orderMessages: [
        {
          avatar: 'https://via.placeholder.com/64',
          title: '订单已完成',
          content: '您的订单123456已完成，欢迎评价。',
          time: '今天 10:30'
        },
        {
          avatar: 'https://via.placeholder.com/64',
          title: '订单已取消',
          content: '您的订单123457已取消，退款将在24小时内到账。',
          time: '昨天 14:20'
        }
      ],
      welfareMessages: [
        {
          avatar: 'https://via.placeholder.com/64',
          title: '领取优惠券',
          content: '您有一张满100减20的优惠券待领取，点击领取。',
          time: '今天 09:15'
        },
        {
          avatar: 'https://via.placeholder.com/64',
          title: '粉丝专属福利',
          content: '感谢您的关注，这是您的粉丝专属福利。',
          time: '三天前 16:45'
        }
      ],
      serviceMessages: [
        {
          avatar: 'https://via.placeholder.com/64',
          title: '系统维护通知',
          content: '系统将于明天凌晨1点进行维护，预计2小时，请提前做好准备。',
          time: '今天 12:00'
        },
        {
          avatar: 'https://via.placeholder.com/64',
          title: '服务升级通知',
          content: '我们的服务已升级，新增了多项功能，欢迎体验。',
          time: '一周前 08:30'
        }
      ]
    }
  },
  methods: {
    clearMessages() {
      this.$toast('消息已清理');
      // Reset messages based on active tab
      switch (this.activeTab) {
        case 'order':
          this.orderMessages = [];
          break;
        case 'welfare':
          this.welfareMessages = [];
          break;
        case 'service':
          this.serviceMessages = [];
          break;
      }
    },
    managePermissions() {
      this.$toast('进入信息管理页面');
      // Navigate to permission management page (to be implemented)
    }
  }
}
</script>

<style scoped>
.message-page {
  height: 100vh;
  background-color: #f5f5f5;
}

.top-nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  background-color: #fff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.tabs {
  display: flex;
}

.tab {
  padding: 10px 20px;
  font-size: 16px;
  color: #666;
  cursor: pointer;
}

.tab.active {
  color: #000;
  font-weight: bold;
  border-bottom: 2px solid #000;
}

.right-buttons {
  display: flex;
  gap: 10px;
}

.btn {
  padding: 6px 12px;
  background-color: #fff;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 14px;
  cursor: pointer;
}

.message-content {
  padding: 10px;
}

.message-list {
  background-color: #fff;
  border-radius: 8px;
}

.message-item {
  display: flex;
  padding: 10px;
  border-bottom: 1px solid #eee;
}

.avatar img {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  margin-right: 10px;
}

.message-info {
  flex: 1;
}

.title {
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 5px;
}

.content {
  font-size: 14px;
  color: #666;
  margin-bottom: 5px;
}

.time {
  font-size: 12px;
  color: #999;
}
</style>