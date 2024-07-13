<script setup lang="ts">
import ChatRoom from './components/ChatRoom.vue'
</script>

<template>
  <div>
    <header> 
      <div>
        <img src="./assets/logo.svg" alt="logo" >
        <h1> WSChat</h1>
      </div>
    </header>
    <ChatRoom/>
  </div>
</template>

<style scoped>
header {
  background-color: #075e54;
  color: white;
  text-align: left;
  border-radius: 20px 20px 0px 0px;
  height: 50px;
  padding: 10px;
}
header div {
  display: flex;
}
header div img {
  width: 50px;
  height: 50px;
  margin: 0px 20px;
  border-radius: 50%;
}
body {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin:0px;
  padding:0px;
  list-style: none;
}
ul > li {
  list-style-type: none;
}

</style>
