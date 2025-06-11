<template>
  <div>
    <h2>WebSocket Chat</h2>
    <input v-model="message" @keyup.enter="sendMessage" placeholder="Escribe un mensaje..." />
    <button @click="sendMessage">Enviar</button>
    <ul>
      <li v-for="(msg, index) in messages" :key="index">{{ msg }}</li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const message = ref('')
const messages = ref([])
let socket = null

const sendMessage = () => {
  if (message.value.trim() !== '' && socket?.readyState === WebSocket.OPEN) {
    socket.send(message.value)
    message.value = ''
  }
}

onMounted(() => {
  socket = new WebSocket('ws://localhost:8000/ws')

  socket.onmessage = (event) => {
    messages.value.push(event.data)
  }

  socket.onclose = () => {
    console.log('WebSocket cerrado')
  }

  socket.onerror = (e) => {
    console.error('WebSocket error', e)
  }
})

onUnmounted(() => {
  if (socket && socket.readyState === WebSocket.OPEN) {
    socket.close()
  }
})
</script>
