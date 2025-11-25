<template>
  <div class="physical-test-page">
    <!-- Test Introduction -->
    <div class="section">
      <div class="l"></div>
      <span class="m">基础健康体测</span>
      <div class="r"></div>
    </div>
    <div class="test-intro">
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

<style lang="scss" scoped>
@import "@/assets/global.scss";

.physical-test-page {
  padding: 0;
}

.section {
  display: flex;
  align-items: center;
  margin: 20rpx 0;
  
  .l, .r {
    flex: 1;
    height: 2rpx;
    background-color: $spLine-color;
  }
  
  .m {
    font-size: 32rpx;
    color: $textBlack-color;
    margin: 0 20rpx;
  }
}

.test-intro {
  text-align: center;
  margin-bottom: 30rpx;

  p {
    font-size: 28rpx;
    color: $textGray-color;
  }
}

.question-list {
  margin-bottom: 30rpx;
}

.question-item {
  background-color: white;
  border-radius: 8rpx;
  padding: 25rpx;
  margin-bottom: 25rpx;
  box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.05);

  h4 {
    font-size: 28rpx;
    margin-bottom: 20rpx;
    color: $textBlack-color;
  }
}

.options {
  display: flex;
  flex-direction: column;
}

.options label {
  margin-bottom: 15rpx;
  font-size: 24rpx;
  cursor: pointer;
  color: $textDarkGray-color;
}

.options input {
  margin-right: 10rpx;
  transform: scale(1.2);
}

.submit-btn {
  width: 90%;
  margin: 0 auto;
  display: block;
  padding: 20rpx;
  background-color: $theme-color;
  color: white;
  border: none;
  border-radius: 30rpx;
  cursor: pointer;
  font-size: 28rpx;
}

.result-dialog {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, $modal-opacity);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}

.dialog-content {
  background-color: white;
  padding: 30rpx;
  border-radius: 8rpx;
  width: 80%;
  max-width: 600rpx;

  h3 {
    font-size: 32rpx;
    margin-bottom: 20rpx;
    text-align: center;
    color: $textBlack-color;
  }

  p {
    font-size: 24rpx;
    margin-bottom: 25rpx;
    line-height: 1.5;
    color: $textDarkGray-color;
    text-align: center;
  }
}

.close-result {
  width: 100%;
  padding: 16rpx;
  background-color: $theme-color;
  color: white;
  border: none;
  border-radius: 30rpx;
  cursor: pointer;
  font-size: 24rpx;
}
</style>