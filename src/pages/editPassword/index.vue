<template>
  <div class="container">
    <div class="header-c">
      <button @click="goBack">返回</button>
      <h2>修改密码</h2>
    </div>
    <div class="content-c">
      <input type="password" v-model="oldPassword" placeholder="请输入原密码">
      <input type="password" v-model="newPassword" placeholder="请输入新密码" @input="checkPasswordStrength">
      <input type="password" v-model="confirmPassword" placeholder="请确认新密码">
      <div class="password-strength" v-if="newPassword.length > 0">
        <div class="strength-bar" :style="{ width: strengthWidth, backgroundColor: strengthColor }"></div>
        <span class="strength-text">{{ strengthText }}</span>
      </div>
      <button class="confirm-btn" @click="confirmPassword">确定</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      oldPassword: '',
      newPassword: '',
      confirmPassword: '',
      strengthText: '',
      strengthColor: '',
      strengthWidth: '0%'
    }
  },
  methods: {
    goBack() {
      wx.navigateBack()
    },
    checkPasswordStrength() {
      const password = this.newPassword
      let strength = 0
      let text = ''
      let color = ''
      let width = '0%'

      // Check password length
      if (password.length >= 6) strength++
      if (password.length >= 8) strength++

      // Check for different character types
      if (/[a-z]/.test(password)) strength++ // Lowercase letters
      if (/[A-Z]/.test(password)) strength++ // Uppercase letters
      if (/[0-9]/.test(password)) strength++ // Numbers
      if (/[^a-zA-Z0-9]/.test(password)) strength++ // Special characters

      // Determine strength level
      if (strength <= 2) {
        text = '弱'
        color = '#FF6B00'
        width = '33%'
      } else if (strength <= 4) {
        text = '中'
        color = '#FFD26B'
        width = '66%'
      } else {
        text = '强'
        color = '#67C23A'
        width = '100%'
      }

      this.strengthText = text
      this.strengthColor = color
      this.strengthWidth = width
    },
    confirmPassword() {
      // Validate form
      if (!this.oldPassword) {
        wx.showToast({ title: '请输入原密码', icon: 'none' })
        return
      }
      if (!this.newPassword) {
        wx.showToast({ title: '请输入新密码', icon: 'none' })
        return
      }
      if (this.newPassword.length < 6) {
        wx.showToast({ title: '新密码长度不能少于6位', icon: 'none' })
        return
      }
      if (this.newPassword !== this.confirmPassword) {
        wx.showToast({ title: '两次输入的密码不一致', icon: 'none' })
        return
      }

      // Save password to local storage (for demo purposes)
      wx.setStorageSync('password', this.newPassword)

      // Show success message and go back
      wx.showToast({ title: '密码修改成功', icon: 'success' })
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
    
    .password-strength {
      margin-bottom: 40rpx;
      
      .strength-bar {
        height: 10rpx;
        border-radius: 5rpx;
        margin-bottom: 10rpx;
      }
      
      .strength-text {
        font-size: 24rpx;
        color: #666;
      }
    }
    
    .confirm-btn {
      width: 100%;
      height: 80rpx;
      background-color: #FF6B00;
      border: none;
      border-radius: 10rpx;
      font-size: 28rpx;
      color: white;
    }
  }
}
</style>