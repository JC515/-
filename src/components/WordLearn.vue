<template>
  <div style="width: 100%; height: 100vh; display: flex; justify-content: center; align-items: center;">
    <div v-if="wordsLoaded">
      <div>
        <p>{{ currentWord.word }}</p>
      </div>
      <div>
        <p>{{ currentWord.pronunciation }}</p>
      </div>
      <div>
        <p>{{ currentWord.definition }}</p>
      </div>
      <div>
        <el-button @click="prevWord" type="primary">上一个</el-button>
        <el-button @click="nextWord" type="primary">下一个</el-button>
      </div>
      <br>
      <br>
      <div>

      </div>
    </div>
    <div v-else>
      <p>单词加载中...</p>
    </div>
  </div>
</template>


<script>
import {defineComponent, ref, onMounted} from 'vue';
import axios from 'axios';

export default defineComponent({
  setup() {
    const words = ref([]);
    const wordsLoaded = ref(false);
    const currentIndex = ref(0);

    onMounted(async () => {
      try {
        const response = await axios.get('http://localhost:8080/word');
        words.value = response.data.data;
        wordsLoaded.value = true;
        updateCurrentWord();
        // window.addEventListener('keydown', this.handleKeyPress);
      } catch (error) {
        console.error(error);
      }
    });
    // const handleKeyPress = (event) => {
    //   switch (event.key) {
    //     case 'ArrowLeft':
    //     case 'a':
    //       prevWord();
    //       break;
    //     case 'ArrowRight':
    //     case 'd':
    //       nextWord();
    //       break;
    //     default:
    //       break;
    //   }
    // };

    const currentWord = ref({
      word: '',
      pronunciation: '',
      definition: '',
    });

    const updateCurrentWord = () => {
      if (words.value.length > 0) {
        currentWord.value = words.value[currentIndex.value];
      }
    };

    const nextWord = () => {
      if (currentIndex.value < words.value.length - 1) {
        currentIndex.value++;
        updateCurrentWord();
      } else {
        alert("已经是最后一个单词了");
      }
    };

    const prevWord = () => {
      if (currentIndex.value > 0) {
        currentIndex.value--;
        updateCurrentWord();
      } else {
        alert("已经是第一个单词了");
      }
    };

    return {
      wordsLoaded,
      currentWord,
      nextWord,
      prevWord,
      is_out_of_order: false,
      // handleKeyPress,
    };
  },
});
</script>
