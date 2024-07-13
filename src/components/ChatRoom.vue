<script setup lang="ts">
import { ref } from 'vue';

const username = ref<string>('')
const isConnect = ref<boolean>(false)
const text = ref<string>('')
const messages = ref<Array<{ username: string; message: string }>>([])

const socket = new WebSocket('ws://localhost:3000')

function handleConnect() {
  if (username.value.length > 0) {
    isConnect.value = true
  }
}

function sendMessage() {
  if (text.value === '') return;
  const data = { username: username.value, message: text.value };

  socket.send(JSON.stringify(data))

  messages.value.push(data)

  text.value = '';
}

socket.onmessage = (e) => {
  const message = JSON.parse(e.data);
  messages.value.push(message);
}
</script>

<template>
  <div class="chat-container">

    <div v-if="!isConnect">
      <div class="modal-background">
        <div class="modal-content">
          <form @submit.prevent="handleConnect">
            <!-- <h3> Enter your name</h3> -->
            <input type="text" v-model="username" placeholder="Enter your name" />
            <br>
            <button type="submit"> Connect </button>
          </form>
        </div>
      </div>
    </div>

    <div v-if="isConnect" class="chat-window">
      <div class="messages-container">
        <div v-for="(val, index) in messages" :key="index"
          :class="[val.username === username ? 'right-bubble' : 'left-bubble']">
          <div v-if="val.username !== username">
            <b>{{ val.username }}</b>
          </div>
          {{ val.message }}
        </div>
      </div>
    </div>

    <div class="chat-input">
      <form @submit.prevent="sendMessage()">
        <input type="text" v-model="text" placeholder="Message" />
        <button type="submit">Send</button>
      </form>
    </div>
  </div>
</template>

<style scoped>
.chat-container {
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.modal-background {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal-content {
  background-color: #128c7e;
  padding: 15px;
  border-radius: 5px;
  box-shadow: 0px 0px 10px #333;
  text-align: center;
  color: white;
}

.modal-content input {
  padding: 10px;
  border-radius: 5px;
  border: none;
  margin-bottom: 10px;
}

.modal-content button {
  padding: 10px;
  border-radius: 5px;
  border: none;
  background-color: #075e54;
  color: white;
}

.chat-window {
  display: flex;
  flex-direction: column;
}

.chat-messages {
  padding: 10px;
  margin-bottom: 10px;
  border-radius: 5px;
}

.chat-input {
  background-color: #f2f2f2;
  padding: 30px;
  display: flex;
  align-items: center;
  position: fixed;
  bottom: 0;
  width: 100%;
}

.chat-input input[type="text"] {
  padding: 15px;
  border: none;
  flex: 1;
  border-radius: 5px 0px 0px 5px;
}

.chat-input button {
  background-color: #128c7e;
  color: #fff;
  padding: 15px;
  border: none;
  border-radius: 0px 5px 5px 0px;
}

.message-container {
  display: inline-block;
  padding: 0 0 0 10px;
  vertical-align: top;
}

.right-bubble {
  background-color: #128c7e;
  border-radius: 10px 10px 0px 10px;
  padding: 10px 15px;
  word-wrap: break-word;
  margin: 2px;
  float: right;
  color: white;
  width: 50%;
}

.left-bubble {
  text-align: left;
  background-color: #075e54;
  border-radius: 10px 10px 10px 0px;
  padding: 10px 15px;
  word-wrap: break-word;
  margin: 2px;
  float: left;
  color: white;
  width: 50%;
}
</style>