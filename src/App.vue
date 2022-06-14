<template>
  <div class="container">
    <main>
      <!-- 头部区域 -->
      <Header @update:keywords="handlerChange"></Header>
      <!-- 搜索结果区域 -->
      <Definition :words="words"></Definition>
    </main>
  </div>
</template>

<script setup>
import Header from './components/Header.vue'
import Definition from './components/Definition.vue'
import { ref } from 'vue'
const words = ref('')
const handlerChange = (value) => {
  // 存储Header中传递过来的搜索关键字
  words.value = value
  // console.log(words.value)
}
</script>

<style lang="less" scoped>
div.container {
  height: 100vh;
  overflow: hidden;
  background-color: #282c34;

  main {
    display: flex;
    flex-direction: column;
    margin: 0 auto;
    padding: 1rem;
    max-width: 1000px;
    height: 100vh;
    color: #fff;
  }
}
</style>
