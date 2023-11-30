<template>
  <div class="word-container">
    <div v-if="wordsLoaded" class="word-card">
      <el-row :gutter="20">
        <el-col :span="24">
          <div class="word-info">
            <p class="word">{{ currentWord.word }}</p>
            <p class="pronunciation">{{ currentWord.pronunciation }}</p>
            <p class="definition">{{ currentWord.definition }}</p>
            <el-button class="play-button" @click="playAudio" circle>
              <el-icon>
                <VideoPlay/>
              </el-icon>
            </el-button>
          </div>
        </el-col>
      </el-row>
      <el-row :gutter="20">
        <el-col :span="12" class="button-col">
          <el-button @click="prevWord" type="primary">上一个</el-button>
        </el-col>
        <el-col :span="12" class="button-col">
          <el-button @click="nextWord" type="primary">下一个</el-button>
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
  background-size: cover;
  background-position: center;
  backdrop-filter: blur(10px);
}

.word-card {
  max-width: 600px;
  width: 80%;
  background: rgba(255, 255, 255, 0.8);
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
  font-family: 'Your Artistic Font', sans-serif;
  font-weight: bold;
  margin-bottom: 5px;
}

.pronunciation {
  font-size: 1.5em;
  font-family: 'Your Artistic Font', sans-serif;
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

.play-button {
  position: absolute;
  top: 40px;
  right: 120px;
}
</style>

<script>
import axios from 'axios';
import {VideoPlay} from '@element-plus/icons-vue';
import bus from "@/Util/EventBus";


export default {
  components: {VideoPlay},
  props: {
    nowUserId: {
      type: String,
      required: true,
    },

  },
  data() {
    let index;
    return {
      words: [],
      wordsLoaded: false,
      currentIndex: 0,
      isOutOfOrder: false,
      currentWord: {
        word: '',
        pronunciation: '',
        definition: '',
      },
      audioType: 1,
      audioPlayer: null,
      index,
    };
  },
  created() {
    bus.on('indexChange', (index) => {
      if (this.index === index)
        return;
      this.updateWordHistory();
    })
  },
  mounted() {
    this.fetchWords();
    this.getWordHistory();
    this.updateCurrentWord();
    window.addEventListener('keydown', this.handleKeyDown);
  },
  beforeUnmount() {
    window.removeEventListener('keydown', this.handleKeyDown);
  },
  beforeDestroy() {
    this.updateWordHistory();
  },
  methods: {
    async fetchWords() {
      try {
        const response = await axios.get('http://localhost:8080/word');
        this.words = response.data.data;
        this.wordsLoaded = true;
        this.updateCurrentWord();
      } catch (error) {
        console.error(error);
      }
    },
    async getWordHistory() {
      try {
        const response = await axios.get('http://localhost:8080/word/history?id=' + this.nowUserId);
        if (response.data.code === 1) {
          this.currentIndex = response.data.data.wordId;
        } else {
          console.log(response.data.message)
        }
      } catch (error) {
        console.error(error);
      }
    },
    async updateWordHistory() {
      try {
        const response = await axios.post('http://localhost:8080/word/history', {
          userId: this.nowUserId,
          wordId: this.currentIndex,
        });
        if (response.data.code === 1) {
        } else {
          console.log(response.data.message)
        }
      } catch (error) {
        console.error(error);
      }
      await this.getWordHistory();
    },
    updateCurrentWord() {
      if (this.words.length > 0) {
        if (this.isOutOfOrder) {
          this.currentIndex = Math.floor(Math.random() * this.words.length);
        }
        this.currentWord = this.words[this.currentIndex];
      }
    },
    nextWord() {
      if (this.isOutOfOrder) {
        this.currentIndex = Math.floor(Math.random() * this.words.length);
      } else if (this.currentIndex < this.words.length - 1) {
        this.currentIndex++;
      } else {
        alert('已经是最后一个单词了');
        return;
      }
      this.updateCurrentWord();
    },
    prevWord() {
      if (this.isOutOfOrder) {
        this.currentIndex = Math.floor(Math.random() * this.words.length);
      } else if (this.currentIndex > 0) {
        this.currentIndex--;
      } else {
        alert('已经是第一个单词了');
        return;
      }
      this.updateCurrentWord();
    },
    handleKeyDown(event) {
      switch (event.key) {
        case 'ArrowLeft':
        case 'a':
        case 'A':
          this.prevWord();
          break;
        case 'ArrowRight':
        case 'd':
        case 'D':
          this.nextWord();
          break;
        default:
          break;
      }
    },
    playAudio() {
      const word = this.currentWord.word;
      const audioType = this.audioType;
      const audioUrl = `https://dict.youdao.com/dictvoice?type=${audioType}&audio=${word}`;

      if (!this.audioPlayer) {
        this.audioPlayer = new Audio();
      }

      this.audioPlayer.src = audioUrl;
      this.audioPlayer.play()
          .then(() => {
            console.log('音频播放已成功启动。');
          })
          .catch((error) => {
            console.error('启动音频播放时出错：', error);
          });
    },
  },
};
</script>
