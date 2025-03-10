<template>
  <div class="hello">
    <img src="@/assets/my-image.jpg" alt="Special Image" class="center-image"/>
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
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.hello {
  text-align: center;
}

.center-image {
  display: block;
  margin: 0 auto;
  max-width: 20%; /* 将图片缩小到原来的1/5 */
}

h1 {
  font-weight: normal;
}

.hearts {
  animation: pulse 1s infinite;
}

@keyframes pulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.1); }
  100% { transform: scale(1); }
}
</style>
