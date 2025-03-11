<template>
  <div class="hello">
    <!-- 左侧花朵 -->
    <div class="flowers left">
      <div
        v-for="n in 10"
        :key="'left-' + n"
        class="flower"
        :style="generateFlowerStyle('left', n)"
      ></div>
    </div>

    <img src="@/assets/my-image.jpg" alt="Special Image" class="center-image"/>

    <!-- 右侧花朵 -->
    <div class="flowers right">
      <div
        v-for="n in 10"
        :key="'right-' + n"
        class="flower"
        :style="generateFlowerStyle('right', n)"
      ></div>
    </div>

    <h1>{{ msg }}</h1>
    <div class="hearts">
      ❤️ ❤️ ❤️
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: '想你了老婆子'
    }
  },
  methods: {
    generateFlowerStyle (side, index) {
      const randomStartY = Math.random() * 100; // 随机的垂直起始位置（0% 到 100%）
      const randomRotation = Math.random() * 360; // 随机初始旋转角度
      const delay = index * 0.3; // 每朵花的动画延迟

      return {
        top: `${randomStartY}%`,
        transform: `rotate(${randomRotation}deg)`,
        animationDelay: `${delay}s`
      }
    }
  }
}
</script>

<style scoped>
/* 父容器居中和样式 */
.hello {
  text-align: center;
  position: relative;
  overflow: hidden;
  height: 100vh;
}

.center-image {
  display: block;
  margin: 0 auto;
  max-width: 20%;
  z-index: 1;
}

h1 {
  font-weight: normal;
  z-index: 1;
  position: relative;
}

.hearts {
  animation: pulse 1s infinite;
}

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
  background-image: url('../assets/flower.jpg'); /* 修正路径 */
  background-size: contain;
  background-repeat: no-repeat;
  opacity: 0;
  animation: fly 5s linear infinite;
}

@keyframes fly {
  0% {
    transform: translateY(100%) rotate(0deg);
    opacity: 0;
  }
  50% {
    opacity: 1;
  }
  100% {
    transform: translateY(0) rotate(360deg);
    opacity: 0;
  }
}
</style>
