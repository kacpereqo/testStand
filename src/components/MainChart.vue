<template>
  <fieldset class="wrapper">
    <legend>Chart</legend>
    <div class="tooltip">
      <ul>
        <li @click="addChart" class="add-chart"><span>+</span></li>
      </ul>
    </div>
    <div class="charts-container" ref="chartsContainer"></div>
  </fieldset>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue'
import * as echart from 'echarts'

interface Chart {
  container: HTMLElement
  canvas: echart.ECharts
}

const time = ref(0)
const dataLimit = 50

const charts = ref<Chart[]>([])
const chartsContainer = ref<HTMLElement | null>()

function resizeCharts() {
  setTimeout(() => {
    charts.value.forEach((chart) => {
      const parentElement = chart.container.parentElement
      const canvasContainer = chart.container.querySelector('div')
      if (!parentElement || !canvasContainer) return

      chart.canvas.resize()
      // console.log(canvasContainer)/
    })
  }, 0)
}

const data = ref<number[]>([])

function pushData() {
  data.value.push(Math.sin(Math.floor(Math.random() * 100)))

  if (data.value.length > dataLimit) {
    data.value.shift()
  }

  const xData = Array.from(
    { length: dataLimit },
    (_, i) => `${time.value > dataLimit ? time.value - dataLimit + i : i}s`,
  )

  time.value++

  charts.value.forEach((chart) => {
    chart.canvas.setOption({
      xAxis: {
        data: xData,
      },
      series: [
        {
          data: data.value,
        },
      ],
    })
  })

  setTimeout(() => {
    pushData()
  }, 1000 / 60)
}

onMounted(() => {
  window.addEventListener('resize', resizeCharts)
  addChart()
  // addChart()
  // addChart()
  pushData()
})

function updateChartsContainerGrid() {
  if (!chartsContainer.value) return
  const lenght = charts.value.length

  let columns
  let rows

  if (lenght === 1) {
    console.log('1 chart')
    columns = 1
    rows = 1
  } else if (lenght === 2) {
    console.log('2 chart')
    columns = 2
    rows = 1
  } else if (length <= 4) {
    console.log('4 chart')
    columns = 2
    rows = 2
  } else if (length === 5) {
    console.log('6 chart')
    columns = 3
    rows = 2
  } else if (length < 9) {
    console.log('9 chart')
    columns = 3
    rows = 3
  } else {
    columns = 4
    rows = 3
  }

  console.log(lenght, columns, rows)

  chartsContainer.value.style.gridTemplateColumns = `repeat(${columns},  minmax(0, 1fr))`
  chartsContainer.value.style.gridTemplateRows = `repeat(${rows},  minmax(0, 1fr))`

  resizeCharts()
}

function addChart() {
  const container = document.createElement('div')
  container.classList.add('chart-container')
  chartsContainer.value?.appendChild(container)

  const chart = echart.init(container)
  chart.setOption({
    tooltip: {},

    xAxis: { data: [] },
    yAxis: {},
    series: [
      {
        type: 'line',
        data: data.value,
      },
    ],
    animation: false,
  })
  charts.value.push({
    container: container,
    canvas: chart,
  })

  updateChartsContainerGrid()
}
</script>

<style>
.chart-container {
  border: 2px solid var(--color-border);
  border-radius: 4px;
  flex-shrink: 1;
}
</style>

<style scoped>
/* container.style.border = '2px solid var(--color-border)'
  container.style.borderRadius = '4px'
  container.style.flex = '1'
  container.style.height = '100%' */

.charts-container {
  flex: 1;
  display: grid;
  gap: 10px;
}

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
