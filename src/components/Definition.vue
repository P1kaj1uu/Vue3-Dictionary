<template>
  <div class="definitions">
    <!-- 当搜索框为空时 -->
    <p class="tip" v-if="!words"> Start by typing a word in search ... </p>
    <!-- 当搜索框搜索结果为空时 -->
    <p class="tip" v-else-if="wordsList.length === 0"> Sorry, the definition of this word was not found. </p>
    <!-- 当搜索结果存在且不为空时 -->
    <div class="definition" v-else v-for="(item, index) in wordsList" :key="index">
      <!-- 音标及读音 -->
      <div class="phonetics" @click="handlerSpeak(index)">
        <span class="phonetic">{{ item?.phonetics[0]?.text }}</span>
        <i class="iconfont icon-yangshengqi"></i>
        <audio :src="item?.phonetics[0]?.audio" hidden controls></audio>
      </div>
      <!-- 含义 -->
      <div class="meanings">
        <div class="meaning" v-for="(meanItem, index) in item.meanings" :key="index">
          <span class="partOfSpeech">{{ meanItem.partOfSpeech }}</span>
          <span class="word-meaning">
            {{ meanItem.definitions.map(Item => Item.definition).join(' ') }}
          </span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { watch, ref, nextTick } from 'vue'
import axios from 'axios'
// 用于存储查询的单词返回的数据
const wordsList = ref([])
// 接收APP组件传递过来的搜索关键字
const props = defineProps({
  words: String
})
// 当点击小喇叭播放时
function handlerSpeak (index) {
  nextTick(() => {
    // 播放对应音频
    document.querySelectorAll('audio')[index].play()
  })
}
async function getData (newValue) {
  console.log(newValue)
  // 下一次请求将上一次的先清空
  wordsList.value = []
  // 发送请求
  await axios.get(`https://api.dictionaryapi.dev/api/v2/entries/en/${newValue}`)
    .then(res => {
      wordsList.value = res.data
      console.log(res)
    })
    .catch(err => console.log(err))
}
// 监听搜索关键字的变化
watch(() => props.words, getData)
</script>

<style lang="less" scoped>
// 修改滚动条样式
::-webkit-scrollbar {
  width: 5px;
  background: #282c34;
}

::-webkit-scrollbar-thumb {
  background: rgba(255, 255, 255, 0.6);
  border-radius: 20px;
}

.definitions {
  border: 10px solid #696969;
  border-radius: 10px;
  height: 65vh;
  padding: 15px;
  overflow-y: scroll;
  display: flex;
  flex-direction: column;
  gap: 5px;

  .tip {
    font-size: 18px;
  }
  .definition {
    margin-bottom: 10px;
    .phonetics {
      display: flex;
      align-items: center;
      margin-bottom: 10px;
      .phonetic {
        cursor: pointer;
        border-radius: 5px;

        margin-right: 10px;
        &:hover {
          opacity: 0.8;
        }
      }
      .iconfont {
        background-image: url('../assets/images/speaker.png');
        width: 16px;
        height: 16px;
        cursor: pointer;
        &:hover {
          opacity: 0.8;
        }
      }
    }
    &:last-child {
      .meanings {
        border-bottom: none;
      }
    }
    .meanings {
      display: flex;
      flex-direction: column;
      gap: 10px;
      padding-bottom: 10px;
      border-bottom: 1px solid #c5bcbc;

      div.meaning {
        .partOfSpeech {
          margin-right: 10px;
          background-color: #3b3e45;
          opacity: 0.8;
          display: inline-block;
          padding-block: 3px;
          padding-inline: 5px;
          font-size: 14px;
          border-radius: 5px;
          line-height: 25px;
        }
        .word-meaning {
          line-height: 25px;
        }
      }
    }
  }
}
</style>
