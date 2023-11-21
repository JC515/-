<template>
  <div class="word-container">
    <div v-if="wordsLoaded" class="word-card">
      <el-row :gutter="20">
        <el-col :span="24">
          <div class="word-info">
            <p class="word">{{ currentWord.word }}</p>
            <p class="pronunciation">{{ currentWord.pronunciation }}</p>
            <p class="definition">{{ currentWord.definition }}</p>
          </div>
        </el-col>
      </el-row>
      <el-row :gutter="20">
        <el-col :span="12" class="button-col">
          <el-button @click="prevWord" type="primary" icon="el-icon-arrow-left">上一个</el-button>
        </el-col>
        <el-col :span="12" class="button-col">
          <el-button @click="nextWord" type="primary" icon="el-icon-arrow-right">下一个</el-button>
        </el-col>
      </el-row>
      <el-row :gutter="20">
        <el-col :span="24">
          <div class="switch-container">
            <el-switch v-model="isOutOfOrder" active-text="乱序" inactive-text="顺序"></el-switch>
          </div>
        </el-col>
      </el-row>
    </div>
    <div v-else>
      <p>单词加载中...</p>
    </div>
  </div>
</template>

<style scoped>
.word-container {
  width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  //background-image: url('path-to-your-background-image.jpg'); /* Set your background image */
  background-size: cover;
  background-position: center;
  backdrop-filter: blur(10px);
}

.word-card {
  max-width: 600px; /* Set your desired maximum width */
  width: 80%; /* Adjust as needed */
  background: rgba(255, 255, 255, 0.8); /* Adjust the opacity as needed */
  border-radius: 10px;
  padding: 20px;
  margin: 20px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
}

.word-info {
  text-align: center;
}

.word {
  font-size: 2em;
  font-family: 'Your Artistic Font', sans-serif; /* Specify your artistic font */
  font-weight: bold;
  margin-bottom: 5px;
}

.pronunciation {
  font-size: 1.5em;
  font-family: 'Your Artistic Font', sans-serif; /* Specify your artistic font */
  margin-bottom: 5px;
}

.definition {
  font-size: 1.2em;
  margin-bottom: 5px;
}

.button-col {
  display: flex;
  justify-content: center;
}

.switch-container {
  text-align: center;
  margin-top: 20px;
}
</style>





<script>
import {defineComponent, ref, onMounted} from 'vue';
import axios from 'axios';

export default defineComponent({
  setup() {
    const words = ref([]);
    const wordsLoaded = ref(false);
    const currentIndex = ref(0);
    const isOutOfOrder = ref(false);

    onMounted(async () => {
      try {
        const response = await axios.get('http://localhost:8080/word');
        words.value = response.data.data;
        wordsLoaded.value = true;
        updateCurrentWord();
      } catch (error) {
        console.error(error);
      }
    });

    const currentWord = ref({
      word: '',
      pronunciation: '',
      definition: '',
    });

    const updateCurrentWord = () => {
      if (words.value.length > 0) {
        if (isOutOfOrder.value) {
          currentIndex.value = Math.floor(Math.random() * words.value.length);
        }
        currentWord.value = words.value[currentIndex.value];
      }
    };

    const nextWord = () => {
      if (isOutOfOrder.value) {
        currentIndex.value = Math.floor(Math.random() * words.value.length);
      } else if (currentIndex.value < words.value.length - 1) {
        currentIndex.value++;
      } else {
        alert('已经是最后一个单词了');
        return;
      }
      updateCurrentWord();
    };

    const prevWord = () => {
      if (isOutOfOrder.value) {
        currentIndex.value = Math.floor(Math.random() * words.value.length);
      } else if (currentIndex.value > 0) {
        currentIndex.value--;
      } else {
        alert('已经是第一个单词了');
        return;
      }
      updateCurrentWord();
    };

    return {
      wordsLoaded,
      currentWord,
      nextWord,
      prevWord,
      isOutOfOrder,
    };
  },
});
</script>
