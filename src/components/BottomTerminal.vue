<template>
  <fieldset class="wrapper">
    <legend class="heading">Terminal</legend>
    <ul class="logs" ref="logs">
      <li v-for="log in messages" :key="log.message">
        <span v-if="log.is_command" style="color: var(--color-accent)"> ></span>
        {{ log.message }}
      </li>
    </ul>
    <div class="input">
      <input type="text" placeholder="Type a command" v-model="input" @keyup.enter="onEnterPress" />
      <input type="submit" value="Send" @click="onEnterPress" />
    </div>
  </fieldset>
</template>

<script setup lang="ts">
import { ref } from 'vue'

interface messageLog {
  message: string
  is_command: boolean
}

const input = ref()
const logs = ref()

function help() {
  messages.value.push({ message: 'Available commands:', is_command: false })
  messages.value.push({ message: 'help - show this message', is_command: false })
  messages.value.push({ message: 'clear - clear the terminal', is_command: false })
}

function scrollBottom() {
  // wait for the next tick to scroll to the bottom
  setTimeout(() => {
    logs.value.scrollTop = logs.value.scrollHeight
  }, 0)
}

function handleCommand(command: string) {
  switch (command) {
    case 'help':
      help()
      break
    case 'clear':
      messages.value = []
      break
    default:
      messages.value.push({ message: `Command not found ${command}`, is_command: false })
  }
}

function onEnterPress() {
  if (!input.value) return
  messages.value.push({ message: input.value, is_command: true })
  handleCommand(input.value)
  scrollBottom()
  input.value = ''
}

const messages = ref<messageLog[]>([
  { message: 'Welcome to the terminal!', is_command: false },
  { message: 'Type `help` to see available commands', is_command: false },
])
</script>

<style scoped>
.wrapper {
  position: relative;
  border-radius: 4px;
  border: 2px solid var(--color-border);
}

.heading {
  background-color: var(--color-background);
  padding: 0 10px;
}

.logs {
  list-style-type: none;
  display: flex;
  flex-direction: column;
  padding: 0;
  margin: 0;
  text-align: left;
  max-height: 200px;
  overflow: auto;
  font-family: monospace;
  scrollbar-width: thin;
}

.input {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-top: 10px;
}
.input input[type='text'] {
  width: 100%;
  padding: 5px;
  border: 1px solid var(--color-border);
  background-color: var(--color-background-mute);
  border-radius: 4px;
  color: var(--color-text);
}

.input input[type='text']:focus {
  outline: none;
}

.input input[type='submit']:hover {
  background-color: var(--color-hover);
}

.input input[type='submit'] {
  padding: 5px 10px;
  border: 1px solid var(--color-border);
  border-radius: 4px;
  background-color: var(--color-accent);
  color: var(--color-text);
  cursor: pointer;
}
</style>
