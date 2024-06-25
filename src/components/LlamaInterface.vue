<template>
  <div class="container">
    <h1>Mistral Interface</h1>
    <textarea v-model="query" placeholder="Entre ta question" class="input"></textarea>
    <button @click="sendQuery" class="button">Envoyer</button>
    <div v-if="response" class="response">
      <h2>Response:</h2>
      <p>{{ response }}</p>
    </div>
    <div id="chatArea">
      <div v-for="message in messages" :key="message.content">
        <ChatMessage :message="message" />
      </div>
      <div v-if="currentOutputMessageContent">
        <ChatMessage :message="{ role: 'agent', content: currentOutputMessageContent }" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import ChatMessage from './ChatMessage.vue';

export default {
  components: {
    ChatMessage,
  },
  data() {
    return {
      query: '',
      response: null,
      messages: [{ role: 'agent', content: 'Salut, comment je peux t\'aider?' }],
      currentOutputMessageContent: ''
    };
  },
  methods: {
    async sendQuery() {
      if (this.query.trim() === '') return;

      this.messages.push({role: 'user', content: this.query});
      try {
        const res = await axios.post('http://127.0.0.1:11434/api/generate', {
          model: 'mistral',
          prompt: this.query,
          stream: false
        });
        this.currentOutputMessageContent = res.data.response;
        this.messages.push({role: 'agent', content: this.currentOutputMessageContent});
        this.response = this.currentOutputMessageContent;
      } catch (error) {
        console.error('Error during the request', error);
      }
      this.query = '';
    },
  },
};
</script>

<style>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 50px;
}

.input {
  width: 80%;
  height: 100px;
  margin-bottom: 20px;
  padding: 10px;
}

.button {
  padding: 10px 20px;
  background-color: #42b983;
  color: white;
  border: none;
  cursor: pointer;
}

.response {
  margin-top: 20px;
  width: 80%;
  text-align: left;
}

#chatArea {
  width: 80%;
  margin-top: 20px;
}

.message {
  margin: 10px 0;
  padding: 10px;
  border-radius: 5px;
}

.message.agent {
  background-color: #d3d3d3;
  text-align: left;
}

.message.user {
  background-color: #42b983;
  color: white;
  text-align: right;
}
</style>
