<template>
  <div id="app">
    <div class="banner">
      <img class="banner-img" src="./assets/imgs/logo.svg">
      <i class="el-icon-plus"></i>
      <img class="banner-img" src="./assets/imgs/logo.png">
      <!-- <span class="banner-text">之初始化</span> -->
      <span class="banner-text">之添加自定义块</span>
    </div>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}

.banner {
  font-size: 40px;
  font-weight: 600;
  height: 80px;
  line-height: 80px;
  margin-bottom: 40px;
  text-align: center;
}

.banner-img {
  width: 80px;
}

.el-icon-plus {
  position: relative;
  top: -24px;
}

.banner-text {
  position: relative;
  top: -24px;
}
</style>
