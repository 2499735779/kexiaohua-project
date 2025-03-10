<template>
  <div class="hello">
    <!-- 左侧花朵 -->
    <div class="flowers left">
      <div v-for="n in 10" :key="'left-' + n" class="flower" :style="{ animationDelay: n * 0.3 + 's' }"></div>
    </div>

    <img src="@/assets/my-image.jpg" alt="Special Image" class="center-image"/>

    <!-- 右侧花朵 -->
    <div class="flowers right">
      <div v-for="n in 10" :key="'right-' + n" class="flower" :style="{ animationDelay: n * 0.3 + 's' }"></div>
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
  data() {
    return {
      msg: '想你了老婆子',
    };
  },
};
</script>

<style scoped>
/* 父容器居中和样式 */
.hello {
  text-align: center;
  position: relative;
  overflow: hidden; /* 防止动画溢出页面 */
  height: 100vh; /* 设置全屏高度 */
}

.center-image {
  display: block;
  margin: 0 auto;
  max-width: 20%; /* 将图片缩小到原来的1/5 */
  z-index: 1; /* 确保图片在最前面 */
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
  background-image: url('@/assets/flower.jpg'); /* 确保路径正确 */
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

/* 飞舞动画路径 */
.left .flower {
  animation: fly-left 5s linear infinite;
}

.right .flower {
  animation: fly-right 5s linear infinite;
}

@keyframes fly-left {
  0% {
    transform: translate(-100%, 100%) rotate(0deg);
    opacity: 0;
  }
  50% {
    transform: translate(0, 50%) rotate(180deg);
    opacity: 1;
  }
  100% {
    transform: translate(50%, 0) rotate(360deg);
    opacity: 0;
  }
}

@keyframes fly-right {
  0% {
    transform: translate(100%, 100%) rotate(0deg);
    opacity: 0;
  }
  50% {
    transform: translate(0, 50%) rotate(180deg);
    opacity: 1;
  }
  100% {
    transform: translate(-50%, 0) rotate(360deg);
    opacity: 0;
  }
}
</style>

