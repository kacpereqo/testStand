<template>
  <fieldset class="wrapper">
    <legend>Chart</legend>
    <div class="add-chart">
      <div class="tooltip">
        <ul>
          <li @click="addChart" class="add-chart"><span>+</span></li>
        </ul>
      </div>
    </div>
  </fieldset>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const charts = ref([])

function addChart() {}
</script>

<style scoped>
.wrapper {
  flex: 1;
  display: flex;
  flex-direction: column;
  border-radius: 4px;
  border: 2px solid var(--color-border);
}

.tooltip {
  display: flex;
  justify-content: end;
}

.tooltip ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

.tooltip li {
  cursor: pointer;
  border-radius: 8px;
  background-color: var(--color-accent);
  color: var(--color-text);
  height: 2rem;
  min-width: 2rem;
  border: 1px solid var(--color-border);
  display: flex;
  justify-content: center;
  align-items: center;
}

.tooltip li:hover {
  background-color: var(--color-hover);
}

legend {
  padding: 0 10px;
}
</style>
