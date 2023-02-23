<template>
  <div class="main">
    <!-- loader -->

    <div class="chat-container">
      <div v-if="loading" class="loader">
        <div class="loader__circle"></div>
        <div class="loader__circle"></div>
        <div class="loader__circle"></div>
        <div class="loader__circle"></div>
        <div class="loader__circle"></div>
        <div class="loader__circle"></div>
        <div class="loader__circle"></div>
        <div class="loader__circle"></div>
      </div>

      <header class="chat-header">Nuxt Text to Image Generator</header>
      <div class="message-display">
        <div v-if="!answer" class="message-bubble">
          Hi, I'm Nuxt Text to image generator. I'm a bot powered by OpenAI's.
          Ask me anything!
        </div>
        <div class="image">
          <img
            v-if="answer"
            width="100%"
            height="100%"
            :src="answer"
            alt="Image"
          />
        </div>
      </div>
      <div class="input-section">
        <input
          v-model="question"
          type="text"
          placeholder="Type your message here..."
        />
        <button @click="sendMessage">Send</button>
      </div>
    </div>
  </div>
</template>

<script>
import { Configuration, OpenAIApi } from "openai";
import { API_KEY } from "@/utils/local.js";
export default {
  data() {
    return {
      loading: false,
      question: "",
      answer: "",
    };
  },
  methods: {
    async sendMessage() {
      this.loading = true;
      const configuration = new Configuration({
        apiKey: API_KEY,
      });
      const openai = new OpenAIApi(configuration);

      const response = await openai.createImage({
        model: "image-alpha-001",
        prompt: this.question,
      });
      this.loading = false;
      this.question = "";
      this.answer = response.data.data[0].url;
    },
  },
};
</script>

<style scoped>
/* Loader */
.loader {
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  top: 50%;
  left: 0;
  z-index: 9999;
}

.loader__circle {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background-color: #007bff;
  margin: 0 5px;
  animation: loader 1s infinite;
}

.loader__circle:nth-child(2) {
  animation-delay: 0.1s;
}

.loader__circle:nth-child(3) {
  animation-delay: 0.2s;
}

.loader__circle:nth-child(4) {
  animation-delay: 0.3s;
}

.loader__circle:nth-child(5) {
  animation-delay: 0.4s;
}

.loader__circle:nth-child(6) {
  animation-delay: 0.5s;
}

.loader__circle:nth-child(7) {
  animation-delay: 0.6s;
}

.loader__circle:nth-child(8) {
  animation-delay: 0.7s;
}

@keyframes loader {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1);
  }
  100% {
    transform: scale(0);
  }
}

.main {
  display: grid;
  place-items: center;
  height: 100vh;
}
.chat-container {
  width: 100%;
  height: 90%;
  max-width: 800px;
  box-shadow: 0 0 10px 0 rgba(0, 0, 0, 0.2);
  padding: 10px;
}

.chat-header {
  text-align: center;
  font-size: 24px;
  font-weight: bold;
  margin: 10px 0px 20px 0px;
}

.message-display {
  height: 80%;
  overflow: hidden;
}

.image {
  height: 100%;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.image img {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
}

.message-bubble {
  width: 95%;
  background-color: #f2f2f2;
  border-radius: 10px;
  padding: 10px;
  margin-bottom: 10px;
}

.input-section {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 20px 10px 10px 10px;
}

input[type="text"] {
  width: 80%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-right: 10px;
}

button {
  width: 15%;
  padding: 10px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

/* Media Queries */
@media (max-width: 500px) {
  .chat-container {
    height: 100%;
  }

  .message-bubble {
    width: 90%;
  }

  input[type="text"] {
    width: 70%;
  }

  button {
    width: 20%;
  }
}
</style>
