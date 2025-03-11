<template>
  <div class="root-container">
    <!-- 花瓣动画 -->
    <div class="flowers left">
      <div
        v-for="flower in leftFlowers"
        :key="flower.key"
        class="flower"
        :style="flower.style"
      ></div>
    </div>

    <div class="flowers right">
      <div
        v-for="flower in rightFlowers"
        :key="flower.key"
        class="flower"
        :style="flower.style"
      ></div>
    </div>

    <!-- 登录界面 -->
    <div class="hello" v-if="!isLoggedIn">
      <div class="login-container">
        <div class="login-card">
          <h2>欢迎登录</h2>
          <form @submit.prevent="login">
            <div class="form-group">
              <label for="username">用户名</label>
              <input
                type="text"
                id="username"
                v-model="username"
                placeholder="请输入用户名"
                required
              />
            </div>
            <div class="form-group">
              <label for="password">密码</label>
              <input
                type="password"
                id="password"
                v-model="password"
                placeholder="请输入密码"
                required
              />
            </div>
            <button type="submit" class="login-button">登录</button>
          </form>
        </div>
      </div>
    </div>

    <!-- 收件箱界面 -->
    <div class="email-container" v-else-if="!showMessage">
      <h2>收件箱</h2>
      <ul class="email-list">
        <li
          class="email-item"
          v-for="conversation in conversations"
          :key="conversation.id"
          @click="openMessage(conversation)"
        >
          <div class="sender">{{ conversation.sender }}</div>
          <div class="subject">{{ conversation.subject }}</div>
          <div class="preview">{{ conversation.preview }}</div>
        </li>
      </ul>
    </div>

    <!-- 信封邮件界面 -->
    <div v-if="showMessage" class="envelope-container">
      <div class="envelope">
        <div class="flap"></div>
        <div class="content">
          <h3>来自 {{ selectedMessage.sender }}</h3>
          <p class="typing-text">{{ animatedText }}</p>
        </div>

      </div>
      <button @click="closeMessage" class="close-button">返回收件箱</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      username: '',
      password: '',
      isLoggedIn: false,
      showMessage: false,
      leftFlowers: [],
      rightFlowers: [],
      conversations: [
        {
          id: 1,
          sender: '氪小化',
          subject: '你有一条未读信息',
          preview: '老婆子想你了！快点点击查看～',
          body: '老婆子想你了，非常想你，希望你每天都开心幸福！',
        },
        {
          id: 2,
          sender: '系统通知',
          subject: '账号安全提醒',
          preview: '为了保证您的账号安全，请及时修改密码。',
          body: '哈哈哈我就知道你会点过来，请退出去查看来自氪小化的信息',
        },
      ],
      selectedMessage: null,
      animatedText: '', // 动态打字文字
    };
  },
  methods: {
    login() {
      if (this.username === '+1' && this.password === '20031108') {
        this.isLoggedIn = true;
      } else {
        alert('账号或密码错误！');
      }
    },
    openMessage(conversation) {
      this.selectedMessage = conversation;
      this.showMessage = true;
      this.startTyping(conversation.body); // 开始打字动画
    },
    closeMessage() {
      this.showMessage = false;
      this.animatedText = ''; // 重置动画文字
    },
    startTyping(text) {
      this.animatedText = ''; // 初始化为空
      let index = 0;
      const typingInterval = setInterval(() => {
        if (index < text.length) {
          this.animatedText += text[index]; // 逐字显示
          index++;
        } else {
          clearInterval(typingInterval); // 停止动画
        }
      }, 100); // 每 100 毫秒显示一个字符
    },
    generateFlowerStyle(side, index) {
      const randomStartY = Math.random() * 100;
      const randomRotation = Math.random() * 360;
      const randomEndX = Math.random() * 200 - 100;
      const delay = index * 0.3;
      const duration = 3 + Math.random() * 20;

      return {
        top: `${randomStartY}%`,
        transform: `rotate(${randomRotation}deg)`,
        animationDelay: `${delay}s`,
        animationDuration: `${duration}s`,
        '--endX': `${randomEndX}%`,
      };
    },
    generateFlowers() {
      for (let i = 0; i < 30; i++) {
        this.leftFlowers.push({
          key: `left-${i}`,
          style: this.generateFlowerStyle('left', i),
        });
      }
      for (let i = 0; i < 20; i++) {
        this.rightFlowers.push({
          key: `right-${i}`,
          style: this.generateFlowerStyle('right', i),
        });
      }
    },
  },
  mounted() {
    this.generateFlowers();
  },
};

</script>

<style scoped>
/* 父容器样式 */
.root-container {
  position: relative;
  overflow: hidden;
  height: 100vh;
  background-color: #fef5f7;
}

/* 花瓣动画样式 */
.flowers {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 100px;
  pointer-events: none;
}

.left {
  left: 0;
}

.right {
  right: 0;
}

.flower {
  position: absolute;
  width: 40px;
  height: 40px;
  background-image: url('../assets/flower.svg');
  background-size: contain;
  background-repeat: no-repeat;
  opacity: 0;
  animation: fly var(--animationDuration, 5s) linear infinite;
}

@keyframes fly {
  0% {
    transform: translateY(100%) rotate(0deg);
    opacity: 0;
  }
  50% {
    transform: translate(calc(var(--endX, 0) * 25), 500%) rotate(180deg);
    opacity: 1;
  }
  100% {
    transform: translate(0, 1000%) rotate(360deg);
    opacity: 0;
  }
}

/* 登录界面样式 */
.login-container {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: 20%;
  left: 20%;
  width: 50%;
  height: 50%;
  background: rgba(255, 182, 193, 0.6);
  border-radius: 15px;
  padding: 30px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
  animation: fadeIn 2s;
}

.login-card h2 {
  font-family: 'Cursive', sans-serif;
  color: #8b008b;
  text-align: center;
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 15px;
}

.form-group input {
  width: 100%;
  padding: 10px;
  border-radius: 5px;
}

.login-button {
  width: 100%;
  padding: 10px;
  background-color: #ff69b4;
  color: white;
  border-radius: 5px;
  cursor: pointer;
}

/* 收件箱样式 */
.email-container {
  padding: 20px;
  background-color: #f4f4f4;
  min-height: 100vh;
}

.email-item {
  background-color: #fff;
  border: 1px solid #e0e0e0;
  margin-bottom: 10px;
  padding: 15px;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.email-item:hover {
  background-color: #fce4ec;
}

.sender {
  font-weight: bold;
  font-size: 16px;
}

.subject {
  color: #8b008b;
  margin-top: 5px;
}

.preview {
  font-size: 12px;
  color: #999;
  margin-top: 5px;
}

/* 信封样式 */
.envelope-container {
  text-align: center;
  padding: 20px;
}

.envelope {
  background-color: #fff0f5;
  padding: 20px;
  border-radius: 12px;
  width: 70%;
  margin: 0 auto;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
  position: relative;
}

.envelope .flap {
  background-color: #ffe4e9;
  height: 40px;
  width: 100%;
  border-radius: 12px 12px 0 0;
  position: absolute;
  top: -40px;
  left: 0;
}

.content h3 {
  font-family: 'Cursive', sans-serif;
  color: #8b008b;
  margin-bottom: 10px;
}
</style>
