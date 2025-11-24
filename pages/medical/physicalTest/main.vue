<template>
  <div class="physical-test-page">
    <!-- Test Introduction -->
    <div class="test-intro">
      <h3>基础健康体测</h3>
      <p>通过简单的问题了解您的健康状况</p>
    </div>

    <!-- Questions -->
    <div class="question-list">
      <div class="question-item" v-for="(question, index) in questions" :key="index">
        <h4>{{ index + 1 }}. {{ question.text }}</h4>
        <div class="options">
          <label v-for="(option, i) in question.options" :key="i">
            <input 
              type="radio" 
              :name="'question' + index" 
              :value="option.value"
              v-model="answers[index]"
            >
            {{ option.text }}
          </label>
        </div>
      </div>
    </div>

    <!-- Submit Button -->
    <button class="submit-btn" @click="submitTest">提交测试</button>

    <!-- Result Dialog -->
    <div class="result-dialog" v-if="showResult">
      <div class="dialog-content">
        <h3>您的体测结果</h3>
        <p>{{ result.text }}</p>
        <button class="close-result" @click="showResult = false">关闭</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      questions: [
        {
          text: '您最近一周睡眠质量如何？',
          options: [
            { text: '很好', value: 10 },
            { text: '一般', value: 7 },
            { text: '较差', value: 4 },
            { text: '很差', value: 1 }
          ]
        },
        {
          text: '您最近一周运动频率如何？',
          options: [
            { text: '每天都运动', value: 10 },
            { text: '每周3-5次', value: 7 },
            { text: '每周1-2次', value: 4 },
            { text: '几乎不运动', value: 1 }
          ]
        },
        {
          text: '您最近一周饮食规律吗？',
          options: [
            { text: '非常规律', value: 10 },
            { text: '比较规律', value: 7 },
            { text: '不太规律', value: 4 },
            { text: '完全不规律', value: 1 }
          ]
        },
        {
          text: '您最近一周是否感到疲劳？',
          options: [
            { text: '从不疲劳', value: 10 },
            { text: '偶尔疲劳', value: 7 },
            { text: '经常疲劳', value: 4 },
            { text: '总是疲劳', value: 1 }
          ]
        }
      ],
      answers: [],
      showResult: false,
      result: {}
    }
  },
  methods: {
    submitTest() {
      if (this.answers.length !== this.questions.length) {
        alert('请回答所有问题')
        return
      }

      const total = this.answers.reduce((sum, answer) => sum + answer, 0)
      const score = total / (this.questions.length * 10) * 100

      let resultText = ''
      if (score >= 80) {
        resultText = '您的健康状况非常好，请继续保持良好的生活习惯！'
      } else if (score >= 60) {
        resultText = '您的健康状况良好，建议适当增加运动和规律饮食。'
      } else if (score >= 40) {
        resultText = '您的健康状况一般，建议调整生活方式，保证充足睡眠和合理饮食。'
      } else {
        resultText = '您的健康状况较差，建议及时就医检查。'
      }

      this.result = { text: resultText }
      this.showResult = true
    }
  }
}
</script>

<style scoped>
.physical-test-page {
  padding: 10px;
}

.test-intro {
  text-align: center;
  margin-bottom: 20px;
}

.test-intro h3 {
  font-size: 18px;
  margin-bottom: 10px;
}

.test-intro p {
  font-size: 14px;
  color: #666;
}

.question-list {
  margin-bottom: 20px;
}

.question-item {
  background-color: #fff;
  border: 1px solid #eee;
  border-radius: 4px;
  padding: 15px;
  margin-bottom: 15px;
}

.question-item h4 {
  font-size: 16px;
  margin-bottom: 15px;
}

.options {
  display: flex;
  flex-direction: column;
}

.options label {
  margin-bottom: 10px;
  font-size: 14px;
  cursor: pointer;
}

.options input {
  margin-right: 5px;
}

.submit-btn {
  width: 100%;
  padding: 10px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}

.result-dialog {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.dialog-content {
  background-color: #fff;
  padding: 20px;
  border-radius: 4px;
  width: 80%;
  max-width: 400px;
}

.dialog-content h3 {
  font-size: 18px;
  margin-bottom: 15px;
}

.dialog-content p {
  font-size: 14px;
  margin-bottom: 15px;
  line-height: 1.5;
}

.close-result {
  width: 100%;
  padding: 8px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}
</style>