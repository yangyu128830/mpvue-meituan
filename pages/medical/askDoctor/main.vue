<template>
  <div class="ask-doctor-page">
    <!-- Doctor List -->
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
        <div class="message doctor-message" v-for="msg in messages" :key="msg.id">
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
        { id: 1, name: '张三', specialty: '内科', hospital: '人民医院', rating: 4.8, avatar: 'https://via.placeholder.com/100' },
        { id: 2, name: '李四', specialty: '外科', hospital: '中医院', rating: 4.7, avatar: 'https://via.placeholder.com/100' },
        { id: 3, name: '王五', specialty: '儿科', hospital: '妇幼保健院', rating: 4.9, avatar: 'https://via.placeholder.com/100' }
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
        this.messages.push({ id: this.messages.length + 1, sender: '我', content: this.inputMessage })
        this.inputMessage = ''
        setTimeout(() => {
          this.messages.push({ id: this.messages.length + 1, sender: '医生', content: '感谢您的咨询，建议您到医院做进一步检查。' })
        }, 1000)
      }
    }
  }
}
</script>

<style scoped>
.ask-doctor-page {
  padding: 10px;
}

.doctor-item {
  display: flex;
  align-items: center;
  padding: 10px;
  margin-bottom: 10px;
  background-color: #fff;
  border: 1px solid #eee;
  border-radius: 4px;
}

.doctor-item img {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  margin-right: 10px;
}

.doctor-info h4 {
  font-size: 16px;
  margin-bottom: 5px;
}

.doctor-info p {
  font-size: 14px;
  margin-bottom: 5px;
}

.doctor-info .hospital {
  font-size: 12px;
  color: #666;
  margin-right: 10px;
}

.doctor-info .rating {
  font-size: 12px;
  color: #ff6600;
}

.consult-btn {
  margin-left: auto;
  padding: 8px 16px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.consultation-dialog {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  flex-direction: column;
}

.dialog-header {
  background-color: #007bff;
  color: white;
  padding: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.close-btn {
  background-color: transparent;
  border: none;
  color: white;
  font-size: 20px;
  cursor: pointer;
}

.chat-area {
  flex: 1;
  background-color: #fff;
  padding: 10px;
  overflow-y: auto;
}

.message {
  margin-bottom: 10px;
  padding: 8px;
  border-radius: 4px;
}

.doctor-message {
  background-color: #f0f0f0;
  align-self: flex-start;
}

.input-area {
  display: flex;
  background-color: #fff;
  padding: 10px;
}

.input-area input {
  flex: 1;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px 0 0 4px;
}

.send-btn {
  padding: 8px 16px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 0 4px 4px 0;
  cursor: pointer;
}
</style>