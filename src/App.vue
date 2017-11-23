<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'APP'
}
</script>

<style lang="scss">
@import '~normalize.css/normalize.css';// 重置浏览器默认样式，保持统一
@import './styles/index.scss'; //全局自定义css样式
</style>
