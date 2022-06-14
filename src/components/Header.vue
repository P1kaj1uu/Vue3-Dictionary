<template>
  <header>
    <h2 class="title">
      <span v-if="keywords.trim().length === 0">Word Hunt</span>
      <span v-else>{{ keywords }}</span>
    </h2>
    <div class="input-wrap">
      <input
        type="text"
        class="input-words"
        v-model="keywords"
        @input="$emit('update:keywords', $event.target.value)"
        placeholder="Search a Word"
        autocomplete="off"
      >
    </div>
  </header>
</template>

<script setup>
import { ref } from 'vue'
const keywords = ref('')
// 子传父
defineEmits(['update:keywords'])
</script>

<style lang="less" scoped>
header {
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  align-items: center;
  height: 30vh;
}
.title {
  font-size: 7vw;
  font-weight: 100;
  @media screen and (max-width: 1000px) {
    font-size: 11vw;
  }
}
.input-wrap {
  input {
    position: relative;
    border: 0;
    border-bottom: 1px solid #c5bcbc;
    width: 50vw;
    padding: 5px;
    outline: none;
    color: #fff;
    background-color: #282c34;
    font-size: 18px;
  }
  i {
    position: absolute;
    display: inline-block;
    background-image: url('../assets/images/delete.png');
    cursor: pointer;
    width: 16px;
    height: 16px;
    right: 25%;
    top: 27%;
  }
  input:focus {
    transition: all .3s;
    border-bottom: 1px solid #fff;
  }
}
</style>
