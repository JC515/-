<template>
  <div class="saying-container">
    <p class="quote">{{ currentQuote.quote }}</p>
    <p class="author">- {{ currentQuote.author }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      quotes: [
        {quote: "It always seems impossible until it's done.", author: "Nelson Mandela"},
        {quote: "The only way to do great work is to love what you do.", author: "Steve Jobs"},
        {
          quote: "Success is not final, failure is not fatal: It is the courage to continue that counts.",
          author: "Winston Churchill"
        },
        {quote: "The secret of getting ahead is getting started.", author: "Mark Twain"},
        {
          quote: "If you really want to do something, you'll find a way. If you don't, you'll find an excuse.",
          author: "Jim Rohn"
        },
        {quote: "The best way to predict the future is to create it.", author: "Peter Drucker"},
        {quote: "The best revenge is massive success.", author: "Frank Sinatra"},
      ],
      currentQuote: {},
      timer: null,
    };
  },
  mounted() {
    this.setRandomQuote();
    this.timer = setInterval(this.setRandomQuote, 4000);
  },
  beforeUnmount() {
    clearInterval(this.timer);
  },
  methods: {
    setRandomQuote() {
      let randomIndex;
      do {
        randomIndex = Math.floor(Math.random() * this.quotes.length);
      } while (this.quotes[randomIndex] === this.currentQuote);

      this.currentQuote = this.quotes[randomIndex];
    },
  },
};
</script>

<style scoped>
.saying-container {
  text-align: center;
  margin-right: 20px;
}

.quote {
  font-size: 1em;
  font-style: italic;
  margin-bottom: 5px;
}

.author {
  font-size: 0.8em;
  font-style: italic;
  text-align: right;
  color: #555;
}
</style>
