<script setup>
import { computed } from 'vue'
import VueApexCharts from 'vue3-apexcharts'
import 'apexcharts/dist/apexcharts.css'

const props = defineProps({
  data: {
    type: Array,
    required: true
  },
  colors: {
    type: Array,
    default: () => ['#3b82f6', '#8b5cf6', '#a855f7', '#6366f1']
  }
})

const chartOptions = computed(() => ({
  chart: {
    type: 'pie',
    toolbar: {
      show: false
    }
  },
  labels: props.data.map(item => item.name),
  colors: props.colors,
  legend: {
    show: false
  },
  dataLabels: {
    enabled: true,
    formatter: (val) => `${val.toFixed(0)}%`
  },
  tooltip: {
    y: {
      formatter: (val) => `${val}%`
    }
  }
}))

const chartSeries = computed(() => {
  return props.data.map(item => item.value)
})
</script>

<template>
  <VueApexCharts
    type="pie"
    height="100%"
    :options="chartOptions"
    :series="chartSeries"
  />
</template>
