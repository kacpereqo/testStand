<template>
  <fieldset class="wrapper">
    <legend>Chart</legend>
    <div id="chart"></div>
  </fieldset>
</template>

<script setup lang="ts"></script>

<style scoped>
.wrapper {
  flex: 1;
  display: flex;
  flex-direction: column;
  border-radius: 4px;
  border: 2px solid var(--color-border);
}

legend {
  padding: 0 10px;
}
</style>
