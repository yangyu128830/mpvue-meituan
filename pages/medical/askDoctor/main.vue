<template>
  <div class="ask-doctor-page">
    <!-- Doctor List -->
    <div class="section">
      <div class="l"></div>
      <span class="m">在线医生</span>
      <div class="r"></div>
    </div>
    <div class="doctor-list">
      <div class="doctor-item" v-for="doctor in doctors" :key="doctor.id">
        <img :src="doctor.avatar" alt="{{ doctor.name }}" />
        <div class="doctor-info">
          <h4>{{ doctor.name }}</h4>
          <p>{{ doctor.specialty }}</p>
          <span class="hospital">{{ doctor.hospital }}</span>
          <span class="rating">★★★★★ ({{ doctor.rating }})</span>
        </div>
        <button class="consult-btn" @click="startConsultation(doctor)">在线咨询</button>
      </div>
    </div>

    <!-- Consultation Dialog -->
    <div class="consultation-dialog" v-if="showConsultation">
      <div class="dialog-header">
        <h3>与{{ currentDoctor.name }}医生对话</h3>
        <button class="close-btn" @click="closeConsultation">×</button>
      </div>
      <div class="chat-area">
        <div :class="['message', msg.type === 'doctor' ? 'doctor-message' : 'user-message']" v-for="msg in messages" :key="msg.id">
          <span class="sender">{{ msg.sender }}:</span>
          <span class="content">{{ msg.content }}</span>
        </div>
      </div>
      <div class="input-area">
        <input type="text" placeholder="请输入您的问题..." v-model="inputMessage" />
        <button class="send-btn" @click="sendMessage">发送</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      doctors: [
        { id: 1, name: '张三', specialty: '内科', hospital: '人民医院', rating: 4.8, avatar: '/static/images/icon_doctor.svg' },
        { id: 2, name: '李四', specialty: '外科', hospital: '中医院', rating: 4.7, avatar: '/static/images/icon_doctor.svg' },
        { id: 3, name: '王五', specialty: '儿科', hospital: '妇幼保健院', rating: 4.9, avatar: '/static/images/icon_doctor.svg' }
      ],
      showConsultation: false,
      currentDoctor: {},
      messages: [],
      inputMessage: ''
    }
  },
  methods: {
    startConsultation(doctor) {
      this.currentDoctor = doctor
      this.showConsultation = true
      this.messages = [
        { id: 1, sender: '医生', content: '您好，请问有什么可以帮您的？' }
      ]
    },
    closeConsultation() {
      this.showConsultation = false
      this.currentDoctor = {}
      this.messages = []
      this.inputMessage = ''
    },
    sendMessage() {
      if (this.inputMessage.trim()) {
        this.messages.push({ id: this.messages.length + 1, sender: '我', content: this.inputMessage, type: 'user' })
        this.inputMessage = ''
        setTimeout(() => {
          this.messages.push({ id: this.messages.length + 1, sender: '医生', content: '感谢您的咨询，建议您到医院做进一步检查。', type: 'doctor' })
        }, 1000)
      }
    }
  }
}
</script>

<style lang="scss" scoped>
@import "@/assets/global.scss";

.ask-doctor-page {
  padding: 0;
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

.doctor-list {
  display: flex;
  flex-direction: column;
  gap: 20rpx;
}

.doctor-item {
  display: flex;
  align-items: center;
  padding: 20rpx;
  background-color: white;
  border-radius: 8rpx;
  box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.05);
  
  img {
    width: 120rpx;
    height: 120rpx;
    border-radius: 50%;
    margin-right: 20rpx;
  }

  .doctor-info {
    flex: 1;
    
    h4 {
      font-size: 32rpx;
      margin-bottom: 10rpx;
      color: $textBlack-color;
    }

    p {
      font-size: 28rpx;
      margin-bottom: 10rpx;
      color: $textDarkGray-color;
    }

    .hospital {
      font-size: 24rpx;
      color: $textGray-color;
      margin-right: 20rpx;
    }

    .rating {
      font-size: 24rpx;
      color: $theme-color;
    }
  }

  .consult-btn {
    padding: 16rpx 24rpx;
    background-color: $theme-color;
    color: white;
    border: none;
    border-radius: 30rpx;
    cursor: pointer;
    font-size: 24rpx;
  }
}

.consultation-dialog {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, $modal-opacity);
  display: flex;
  flex-direction: column;
  z-index: 9999;
}

.dialog-header {
  background-color: $theme-color;
  color: white;
  padding: 20rpx;
  display: flex;
  justify-content: space-between;
  align-items: center;
  
  h3 {
    font-size: 32rpx;
  }
}

.close-btn {
  background-color: transparent;
  border: none;
  color: white;
  font-size: 36rpx;
  cursor: pointer;
}

.chat-area {
  flex: 1;
  background-color: white;
  padding: 20rpx;
  overflow-y: auto;
}

.message {
  margin-bottom: 20rpx;
  padding: 16rpx;
  border-radius: 8rpx;
  max-width: 70%;
  
  .sender {
    font-weight: bold;
    margin-right: 10rpx;
    color: $textBlack-color;
  }
  
  .content {
    color: $textDarkGray-color;
  }
}

.doctor-message {
  background-color: #f0f0f0;
  align-self: flex-start;
  margin-right: auto;
}

.user-message {
  background-color: $theme-color;
  align-self: flex-end;
  margin-left: auto;
  
  .sender {
    color: white;
  }
  
  .content {
    color: white;
  }
}

.input-area {
  display: flex;
  background-color: white;
  padding: 20rpx;
}

.input-area input {
  flex: 1;
  padding: 16rpx;
  border: 2rpx solid $spLine-color;
  border-radius: 30rpx 0 0 30rpx;
  font-size: 24rpx;
}

.send-btn {
  padding: 16rpx 32rpx;
  background-color: $theme-color;
  color: white;
  border: none;
  border-radius: 0 30rpx 30rpx 0;
  cursor: pointer;
  font-size: 24rpx;
}
</style>