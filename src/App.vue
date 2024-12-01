<template>
  <MainChart />
  <BottomTerminal />
</template>

<script setup lang="ts">
import MainChart from './components/MainChart.vue'
import BottomTerminal from './components/BottomTerminal.vue'
</script>

<style>
#app {
  display: flex;
  flex-direction: column;
  height: 100vh;
  gap: 10px;
}
</style>
