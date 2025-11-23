<template>
  <div class="container">
    <div class="header-c">
      <button @click="goBack">返回</button>
      <h2>修改昵称</h2>
    </div>
    <div class="content-c">
      <input type="text" v-model="nickname" placeholder="请输入新昵称">
      <button @click="generateNickname">AI智能推荐昵称</button>
      <div class="recommendations" v-if="recommendations.length > 0">
        <h3>推荐昵称：</h3>
        <div class="recommendation" v-for="(item, index) in recommendations" :key="index" @click="selectNickname(item)">{{ item }}</div>
      </div>
      <button class="confirm-btn" @click="confirmNickname">确定</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      nickname: '',
      recommendations: []
    }
  },
  methods: {
    goBack() {
      wx.navigateBack()
    },
    generateNickname() {
      // AI nickname recommendation logic
      const adjectives = ['阳光', '快乐', '勇敢', '聪明', '温柔', '帅气', '可爱', '活泼', '善良', '幽默']
      const nouns = ['少年', '女孩', '天使', '精灵', '骑士', '公主', '冒险家', '梦想家', '艺术家', '旅行者']
      const recommendations = []
      
      for (let i = 0; i < 5; i++) {
        const adj = adjectives[Math.floor(Math.random() * adjectives.length)]
        const noun = nouns[Math.floor(Math.random() * nouns.length)]
        recommendations.push(adj + noun)
      }
      
      this.recommendations = recommendations
    },
    selectNickname(nickname) {
      this.nickname = nickname
    },
    confirmNickname() {
      if (!this.nickname) {
        wx.showToast({ title: '请输入昵称', icon: 'none' })
        return
      }
      
      // Save nickname to local storage
      wx.setStorageSync('nickname', this.nickname)
      
      // Show success message and go back
      wx.showToast({ title: '昵称修改成功', icon: 'success' })
      setTimeout(() => {
        wx.navigateBack()
      }, 1500)
    }
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
  
  .content-c {
    padding: 40rpx 20rpx;
    
    input {
      width: 100%;
      height: 80rpx;
      border: 2rpx solid #f0f0f0;
      border-radius: 10rpx;
      padding: 0 20rpx;
      font-size: 28rpx;
      margin-bottom: 20rpx;
    }
    
    button {
      width: 100%;
      height: 80rpx;
      background-color: #FFD26B;
      border: none;
      border-radius: 10rpx;
      font-size: 28rpx;
      color: white;
      margin-bottom: 20rpx;
    }
    
    .recommendations {
      margin-bottom: 40rpx;
      
      h3 {
        font-size: 28rpx;
        color: #333;
        margin-bottom: 10rpx;
      }
      
      .recommendation {
        font-size: 26rpx;
        color: #666;
        padding: 10rpx 0;
        border-bottom: 1rpx solid #f0f0f0;
        cursor: pointer;
      }
    }
    
    .confirm-btn {
      background-color: #FF6B00;
    }
  }
}
</style>